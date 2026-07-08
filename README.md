# Rumble Chatters Goal Monitor

A Chrome Extension and lightweight backend service for helping live Rumble creators reach the monthly **Total unique chatters** goal.

The extension reads the creator's own Rumble dashboard locally, checks whether they are currently live using their local Rumble Livestreaming API URL, and reports only safe derived status to the shared backend at:

```text
https://chatters.streemrz.com
```

The backend shows a live list of creators who are currently streaming and still need chatters.

---

## What the app does

The app tracks this Rumble dashboard metric:

```text
Total unique chatters this month
```

Example:

```text
187 / 75 chatters
```

Where:

- `187` is the creator's current monthly unique chatters count.
- `75` is the target goal.
- If the current value is below the target, the goal is red / incomplete.
- If the current value meets or exceeds the target, the goal is green / achieved.

The extension only displays creators in the shared list when all of these are true:

```text
The creator is live
AND the creator is below the chatters goal
AND the creator has reporting enabled
```

Once a creator reaches the goal, their stream automatically disappears from the public “Live users needing chatters” list.

---

## Privacy and security

The user's Rumble Livestreaming API URL stays local in their Chrome extension.

It is **not** uploaded to the backend.

The backend does not store:

- Rumble API URLs
- Rumble API keys
- Rumble account passwords
- Rumble dashboard cookies
- Rumble session data

The backend receives only derived reporting data, such as:

```json
{
  "username": "ExampleUser",
  "display_name": "ExampleUser",
  "profile_pic_url": "https://example.com/avatar.jpg",
  "is_live": true,
  "livestream_id": "abc123",
  "livestream_title": "Example Live Stream",
  "canonical_url": "https://rumble.com/example.html",
  "embed_url": "https://rumble.com/embed/vabc123",
  "chatters_current": 42,
  "chatters_goal": 75,
  "chatters_achieved": false
}
```

All production traffic should use HTTPS.

---

## Chrome Extension install guide

1. Download and extract the extension ZIP.
2. Open Chrome.
3. Go to:

```text
chrome://extensions
```

4. Enable **Developer mode**.
5. Click **Load unpacked**.
6. Select the extracted folder:

```text
rumble_chatters_extension
```

7. Pin the extension to the Chrome toolbar.

---

## Extension configuration

Open the extension options page.

You only need to configure:

### Rumble Livestreaming API URL

Paste your full private Rumble Livestreaming API URL.

This remains local in your browser.

### Display name override

Optional.

Leave blank to use your Rumble API username.

### Enable scheduled 5-minute reporting

Recommended.

When enabled, the extension checks your local dashboard/API state every 5 minutes and reports your derived status to the backend.

### Goal achieved audio

Optional.

You can choose a local audio clip that plays when your stream crosses from below-goal to achieved-goal.

The audio file stays local in the extension and is not uploaded.

Recommended settings:

- Short clip: 1–5 seconds
- Format: MP3, WAV, or OGG
- Volume: start at 40–60%

The extension includes a once-per-stream/month guard so the sound does not play every 5 minutes after the goal is achieved.

---

## Normal user workflow

### 1. Open your Rumble earnings dashboard

Open:

```text
https://rumble.com/account/dashboard?type=earnings&interval=30
```

The extension reads the visible `Total unique chatters this month` value from this page.

### 2. Open the extension popup

Click the extension icon.

You should see:

- your local chatters count
- your goal
- your percentage progress
- whether the goal is achieved
- a refresh button
- live users who currently need chatters

### 3. Go live on Rumble

When your Rumble Livestreaming API shows that you are live, the extension reports your live status.

### 4. Below goal

If you are live and below the goal, you appear in the shared list.

Other users can see:

- your avatar
- username
- current chatter count
- target goal
- live stream title
- link to watch the stream

### 5. Goal achieved

When you meet or exceed the goal:

- the extension marks the goal achieved
- your local success audio can play
- the backend removes your stream from the shared list
- the stream link disappears from the list

---

## Popup behaviour

The popup has three main areas.

### Your chatter goal

Shows the local dashboard result, for example:

```text
187 / 75
Goal achieved
100%
```

When below target, it shows how many more chatters are needed.

### Live users needing chatters

Shows currently live creators who are still below their goal.

The list is sorted from lowest progress to highest progress, so the most needy streams appear first.

The popup shows up to 3 stream cards at a time, then scrolls inside the list.

Each stream card can show:

- avatar
- username
- stream title
- current count vs target
- number still needed
- live badge
- watch button

### Footer

Shows:

- Options link
- connected backend host

---

## Backend behaviour

The backend is hosted at:

```text
https://chatters.streemrz.com
```

The backend exposes REST endpoints for:

- health checks
- receiving signed extension reports
- listing live users who need chatters

The backend only returns users where:

```text
is_live = true
AND chatters_current < chatters_goal
```

Users who are offline or have achieved the target are hidden.

---

## Backend health check

To test the backend:

```bash
curl https://chatters.streemrz.com/health
```

Expected result:

```json
{
  "ok": true,
  "service": "Rumble Chatters Backend",
  "env": "production"
}
```

---

## Lightsail / Debian service notes

The FastAPI app should run privately on:

```text
127.0.0.1:8011
```

Nginx should proxy public HTTPS traffic to the local backend.

Open only these inbound ports on Lightsail:

```text
22   SSH
80   HTTP
443  HTTPS
```

Do not expose port `8011` publicly.

Useful commands:

```bash
sudo systemctl status rumble-chatters --no-pager
sudo journalctl -u rumble-chatters -f
sudo systemctl restart rumble-chatters
sudo systemctl reload nginx
```

---

## Troubleshooting

### The extension shows no local chatters value

Open the Rumble earnings dashboard first:

```text
https://rumble.com/account/dashboard?type=earnings&interval=30
```

Then click **Refresh** in the extension popup.

The extension needs the dashboard page to be available so it can read the visible chatters metric.

### The dashboard value is wrong or missing

Check that the Rumble page contains the text:

```text
Total unique chatters this month
```

Rumble UI changes may require an extension parser update.

### I am live but not appearing in the list

Check:

- your Rumble Livestreaming API URL is configured
- scheduled reporting is enabled
- you are actually live according to the Rumble API
- your current chatters value is below the goal
- the extension has recently refreshed or reported

You will not appear if your goal is already achieved.

### My avatar is missing

The popup tries multiple avatar fields:

- `profile_pic_url`
- `avatar_url`
- `profile_image_url`
- nested profile/avatar fields
- profile page Open Graph image fallback where available

If no usable image is found, the popup shows initials.

### Audio did not play

Check:

- an audio file is configured
- the volume is above zero
- the goal crossed from below-goal to achieved-goal
- it has not already played for the same stream/month

Use **Test Audio** in Options to confirm the clip works.

### Backend health works but extension list is empty

That usually means no live users currently need chatters.

The list only shows creators who are:

```text
live
AND below target
```

---

## Recommended usage guidance

For creators:

1. Install the extension.
2. Add your local Rumble Livestreaming API URL.
3. Open your Rumble earnings dashboard before or during a stream.
4. Keep scheduled reporting enabled.
5. Add a short success sound if desired.
6. When live and below target, you will appear in the shared list.
7. When you reach the goal, you automatically disappear from the list.

For viewers/supporters:

1. Open the extension popup.
2. Look at the live users needing chatters list.
3. Prioritise the users at the top.
4. Click **Watch**.
5. Join the stream chat to help them reach the goal.

---

## Current app rules

```text
Goal target source:
  Rumble dashboard metric

Live status source:
  local Rumble Livestreaming API URL

Backend stores:
  derived public status only

Backend does not store:
  Rumble API URL or key

List visibility:
  live users below goal only

List sorting:
  lowest progress first

List display:
  maximum 3 cards visible, then internal scroll

Achievement behaviour:
  play local audio once per stream/month
  remove achieved user from public list
```

---

## Version notes

This README describes the extension build with:

```text
Hardcoded backend: chatters.streemrz.com
Logo/icon support
Goal-achieved local audio
Redesigned popup UI
Live list sorting and scrolling
Avatar fallback handling
```

# Rumble Chatters Goal Monitor — Version 2

A Chrome extension that helps live Rumble creators track and reach the monthly **Total unique chatters** goal.

The extension reads the creator's own Rumble dashboard locally, checks whether they are live through their private Rumble Livestreaming API URL, and shares only the derived status needed to display creators who are currently live and still below their goal.

Service information:

```text
https://chatters.streemrz.com
```

---

## What the extension does

Rumble Chatters tracks this Rumble dashboard metric:

```text
Total unique chatters this month
```

Example:

```text
42 / 75 chatters
```

A creator appears in **Live users needing chatters** only when:

```text
The creator is currently live
AND the creator is below the chatter goal
AND scheduled reporting is enabled
AND the browser installation is authenticated and enrolled
```

When the creator reaches the goal or goes offline, their stream is automatically removed from the shared list.

---

## Version 2 features

- **Open Dashboard** and **Open Options** buttons at the top of the popup.
- Secure authentication through a Streemrz.com account.
- Per-browser cryptographic device identity.
- No permanent shared API secret inside the extension.
- Automatic five-minute reporting.
- Automatic first-of-month reset using New York time.
- Optional local goal-achieved audio.
- Live-user list sorted from lowest progress upward.
- **Version 2** footer with a link to `streemrz.com`.

![Rumble Chatters Version 2 popup](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/02-extension-popup.png)

---

# Installation

## Requirements

Before installing, you need:

- Google Chrome or another compatible Chromium browser that supports unpacked Manifest V3 extensions.
- A Streemrz.com account.
- Access to your own Rumble earnings dashboard.
- Your private Rumble Livestreaming API URL.

Your Rumble API URL must:

- use HTTPS;
- be hosted on `rumble.com`; and
- belong to you.

---

## Fresh installation

### 1. Download and extract the extension

Download the latest full extension ZIP from the project release page.

Extract it to a permanent folder.

Do not load the extension from a temporary extraction folder that may later be deleted or moved. Chrome needs continued access to the folder containing `manifest.json`.

### 2. Open Chrome Extensions

Open:

```text
chrome://extensions
```

Then:

1. Enable **Developer mode**.
2. Click **Load unpacked**.
3. Select the extracted folder containing `manifest.json`.
4. Pin **Rumble Chatters** to the Chrome toolbar.

![Loading the unpacked Chrome extension](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/01-chrome-load-unpacked.png)

### 3. Open Options

Click the extension icon, then select **Open Options**.

The extension uses the fixed backend:

```text
https://chatters.streemrz.com
```

The backend address cannot be changed to an arbitrary service.

### 4. Authenticate with Streemrz

Click:

```text
Authenticate with Streemrz
```

The extension opens the normal Streemrz.com login and consent page.

- When you are already signed into Streemrz, the existing browser session is reused.
- Otherwise, Streemrz asks for your username, password and configured MFA.
- Streemrz then displays an approval page for Rumble Chatters.
- Press **Authorize** to bind this browser installation to your account.

The extension never receives your password or MFA response.

![Authenticate with Streemrz button in Options](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/03-extension-options-authenticate.png)

![Streemrz Rumble Chatters consent screen](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/04-streemrz-consent.png)

After approval, Options should show:

```text
Streemrz account
Authenticated

Secure device connection
Enrolled

private key extractable: no
```

![Authenticated Streemrz account and enrolled device](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/05-extension-authenticated.png)

### 5. Add your Rumble Livestreaming API URL

Paste your complete private Rumble Livestreaming API URL into:

```text
Rumble Livestreaming API URL
```

This value stays inside your browser extension and is not uploaded to the Chatters service.

![Rumble Livestreaming API URL configuration](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/06-rumble-api-configuration.png)

### 6. Configure optional settings

#### Display name override

Leave blank to use the name returned by the Rumble API, or enter a preferred display name.

#### Scheduled five-minute reporting

Keep this enabled for normal use.

When enabled, the extension periodically:

1. checks the local Rumble dashboard value;
2. checks your current live status;
3. creates a securely signed status report; and
4. sends the derived status to the shared service.

#### Goal-achieved audio

You may select a local MP3, WAV or OGG file to play when the goal is reached.

Recommended settings:

- clip length: 1–5 seconds;
- starting volume: 40–60%; and
- **Play once per stream/month** enabled.

The audio file stays local and is never uploaded.

![Goal achieved audio settings](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/07-goal-audio-settings.png)

### 7. Save and test

Click:

```text
Save & Test
```

A successful result confirms that the configuration was saved and a secure report was accepted.

### 8. Open the Rumble earnings dashboard

Open:

```text
https://rumble.com/account/dashboard?type=earnings&interval=30
```

The extension reads the visible **Total unique chatters this month** metric from this page.

After opening the dashboard for the first time, press the refresh button in the extension popup.

---

## Upgrading an existing unpacked installation

When an update is supplied as a differential patch:

1. Back up the current extension folder.
2. Extract the differential ZIP.
3. Copy its files over the existing extension folder.
4. Do **not** uninstall the extension.
5. Open `chrome://extensions`.
6. Press **Reload** on Rumble Chatters.
7. Open Options.
8. Complete any newly requested authentication step.
9. Press **Save & Test**.

Not uninstalling helps preserve:

- the installation identity;
- the non-extractable private key;
- the Rumble API URL;
- the selected audio clip; and
- extension preferences.

---

# Using Rumble Chatters

## Popup controls

The two primary actions are displayed at the top:

- **Open Dashboard** — opens the Rumble earnings dashboard.
- **Open Options** — opens authentication and configuration.

The refresh button performs an immediate local check and report.

## Your chatter goal

The popup displays:

- current monthly chatter count;
- goal;
- percentage progress;
- number still required; and
- achieved or incomplete status.

Example:

```text
42 / 75
33 more unique chatters needed
56%
```

## Live users needing chatters

Only creators who are live and below target are shown.

The list is sorted from the lowest percentage progress to the highest, helping users prioritise creators who need the most support.

Each card may show:

- avatar;
- Rumble username or display name;
- stream title;
- current count and target;
- number still needed;
- live badge; and
- **Watch** button.

Once a creator reaches the goal or goes offline, their stream is removed from the list.

---

## Monthly reset

The local monthly chatter state resets at:

```text
00:00 America/New_York on the first day of each month
```

This automatically handles both EST and EDT.

At the reset boundary, the extension:

1. changes the displayed chatter count to `0`;
2. clears the previous month's cached result;
3. reports the new monthly state;
4. reloads an open Rumble earnings dashboard without using its cache; and
5. rescans the dashboard shortly afterward.

The extension also checks for a missed rollover when Chrome starts, the extension updates, a scheduled report runs, or the dashboard provides a new metric.

A stale dashboard result from the previous month is rejected rather than restoring the old count.

---

# Account and device controls

## Re-authenticate with Streemrz

Use **Re-authenticate with Streemrz** to confirm or refresh the current account connection.

## Disconnect account

**Disconnect account** removes the Streemrz account binding from that browser installation.

Reporting will not continue until the browser is authenticated again.

---

# Privacy and security

## Data that remains local

The following remains in your browser:

- private Rumble Livestreaming API URL;
- Rumble API key contained in that URL;
- local goal-achieved audio file;
- non-extractable device private key;
- extension preferences; and
- Rumble dashboard cookies and session data.

## Data never given to the extension

The extension never receives or stores:

- Streemrz password;
- MFA response;
- Streemrz session cookie;
- backend OAuth client secret; or
- browser login credentials.

Authentication is handled on Streemrz.com.

## Data used by the shared service

The shared service stores only the account, device and derived reporting information needed to authenticate installations and operate the live-user list.

This may include:

- Streemrz user ID and username;
- browser installation and extension identifiers;
- device public-key information;
- account and device status;
- last reporting time; and
- derived Rumble live and chatter status.

The shared service does **not** store:

- private Rumble API URL;
- Rumble API key;
- Rumble password;
- Streemrz password;
- MFA code;
- dashboard cookies; or
- browser session data.

## Device security

Each browser installation has its own cryptographic identity.

The private key is generated locally and stored as non-extractable. Requests are signed so the service can reject invalid, expired or replayed reports.

There is no permanent global API secret embedded in Version 2 of the extension.

---

# Troubleshooting

## Authentication was cancelled

Close the cancelled-authentication tab and press **Authenticate with Streemrz** again.

Every attempt creates a new short-lived authentication session. A failed or cancelled attempt cannot be resumed.

## Authentication does not complete

Try the following:

1. Close the failed authentication tab.
2. Return to Rumble Chatters Options.
3. Press **Authenticate with Streemrz** again.
4. Confirm that cookies and pop-ups are not being blocked for `streemrz.com`.
5. Temporarily disable an ad blocker or privacy extension for the authentication page.
6. Confirm that you completed the consent screen by pressing **Authorize**.

Do not paste passwords, MFA codes, OAuth links or private API URLs into a public support issue.

## Options says authenticated but the popup list fails

Confirm:

- **Secure device connection** says **Enrolled**;
- the Streemrz account says **Authenticated**;
- **Save & Test** succeeds;
- the browser extension was reloaded after upgrading; and
- your internet connection can reach `chatters.streemrz.com`.

## The extension shows no local chatter value

1. Open the Rumble earnings dashboard:

   ```text
   https://rumble.com/account/dashboard?type=earnings&interval=30
   ```

2. Wait for the page to load fully.
3. Open the extension popup.
4. Press refresh.

The page must contain:

```text
Total unique chatters this month
```

A future Rumble interface change may require an extension update.

## I am live but not shown in the shared list

Confirm:

- the Rumble API URL is present and valid;
- the URL uses HTTPS and belongs to `rumble.com`;
- scheduled reporting is enabled;
- Streemrz authentication is complete;
- the device is enrolled;
- Rumble reports you as live;
- the current chatter count is below the goal; and
- a recent refresh or scheduled report has completed.

A creator is not listed after the goal is achieved.

## The live-user list is empty

This normally means no currently reporting creator is both:

```text
live
AND below target
```

## Audio does not play

Confirm:

- an audio file is selected;
- volume is above zero;
- **Play audio when goal is achieved** is enabled;
- the goal changed from below target to achieved; and
- the clip has not already played for that stream/month.

Use **Test Audio** in Options.

## Avatar is missing

The extension attempts common Rumble avatar fields and may fall back to initials when no safe image is available.

---

# Adding screenshots

Store public screenshots under:

```text
docs/images/
```

Suggested filenames:

```text
01-chrome-load-unpacked.png
02-extension-popup.png
03-extension-options-authenticate.png
04-streemrz-consent.png
05-extension-authenticated.png
06-rumble-api-configuration.png
07-goal-audio-settings.png
```

Before publishing a screenshot, hide or crop:

- the private Rumble Livestreaming API URL;
- activation or recovery codes;
- OAuth query strings or callback URLs containing `code` or `state`;
- backend client secrets;
- browser credential identifiers;
- email addresses or other personal information you do not intend to publish.

Usernames and extension IDs are not passwords, but they should still be shown only when intentionally included.

---

# Version 2 highlights

- Streemrz account authentication.
- Secure per-browser device identity.
- Signed reporting with replay protection.
- First-of-month New York reset and rescan.
- Prominent **Open Dashboard** and **Open Options** controls.
- Local goal-achieved audio.
- Improved popup layout, live-list sorting and avatar fallbacks.
- **Version 2** footer with a Streemrz link.

---

## Streemrz

Project and service information:

```text
https://streemrz.com
```

# Rumble Chatters Goal Monitor — Version 2

A Chrome extension and lightweight backend service that helps live Rumble creators reach the monthly **Total unique chatters** goal.

The extension reads the creator's own Rumble dashboard locally, checks live status through the creator's private Rumble Livestreaming API URL, and sends only derived status information to:

```text
https://chatters.streemrz.com
```

The shared popup list shows Rumble creators who are currently live and still below their monthly chatter goal.

---

## Current matched release

| Component | Version |
|---|---:|
| Chrome extension | `2.2.0` |
| Rumble Chatters backend | `0.8.0a` or later |
| Streemrz.com integration | `p4e633` or later |
| Extension footer | **Version 2** |

The normal activation path now requires the user to authenticate with a Streemrz.com account. Manual activation codes are retained only as an administrator/support fallback during migration.

---

## What the extension does

The extension tracks the Rumble dashboard metric:

```text
Total unique chatters this month
```

Example:

```text
42 / 75 chatters
```

Where:

- `42` is the creator's current monthly unique-chatters count.
- `75` is the target goal.
- Below target, the goal is shown as incomplete.
- At or above target, the goal is shown as achieved.

A creator appears in **Live users needing chatters** only when:

```text
The creator is currently live
AND the creator is below the chatter goal
AND scheduled reporting is enabled
AND the browser installation is securely enrolled
```

When the creator reaches the goal, their stream automatically disappears from the shared list.

---

## Main Version 2 features

- **Open Dashboard** and **Open Options** buttons at the top of the popup.
- Streemrz account authentication and explicit consent.
- Per-browser cryptographic device identity.
- No permanent shared API secret inside the extension.
- Five-minute scheduled reporting.
- Automatic first-of-month reset using New York time.
- Local goal-achieved audio.
- Live-user list sorted from lowest progress upward.
- **Version 2** footer with a link to `streemrz.com`.

![Rumble Chatters Version 2 popup](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/02-extension-popup.png)

---

# User installation

## Requirements

Before installing, the user needs:

- Google Chrome or another compatible Chromium browser that supports unpacked Manifest V3 extensions.
- A Streemrz.com account.
- Access to their own Rumble earnings dashboard.
- Their private Rumble Livestreaming API URL.

The Rumble API URL must:

- use HTTPS;
- be hosted on `rumble.com`; and
- belong to the user installing the extension.

---

## Fresh installation

### 1. Download and extract the extension

Use the complete extension package:

```text
Rumble_Chatters_Extension_v2.2.0_Full.zip
```

Extract it to a permanent folder.

Do not load the extension from a temporary Downloads extraction folder that may later be deleted or moved. Chrome needs continued access to the folder containing `manifest.json`.

### 2. Open Chrome Extensions

Open:

```text
chrome://extensions
```

Then:

1. Enable **Developer mode**.
2. Click **Load unpacked**.
3. Select the extracted extension folder containing `manifest.json`.
4. Pin **Rumble Chatters** to the Chrome toolbar.

![Loading the unpacked Chrome extension](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/01-chrome-load-unpacked.png)

### 3. Open Options

Click the extension icon, then select **Open Options**.

The backend address is fixed to:

```text
https://chatters.streemrz.com
```

The user cannot redirect the extension to an arbitrary backend.

### 4. Authenticate with Streemrz

Click:

```text
Authenticate with Streemrz
```

The extension opens the normal Streemrz.com authentication and consent page.

- When already signed into Streemrz, the existing browser session is reused.
- Otherwise Streemrz asks for the user's username, password and configured MFA.
- Streemrz then displays an approval page for Rumble Chatters.
- The user must press **Authorize** to bind this browser installation.

The extension never receives the user's password or MFA response.

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

### 5. Add the Rumble Livestreaming API URL

Paste the user's complete private Rumble Livestreaming API URL into:

```text
Rumble Livestreaming API URL
```

This value stays inside the browser extension and is not uploaded to the Chatters backend.

![Rumble Livestreaming API URL configuration](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/06-rumble-api-configuration.png)

### 6. Configure optional settings

#### Display name override

Leave blank to use the name returned by the Rumble API, or enter a preferred display name.

#### Scheduled five-minute reporting

Keep this enabled for normal use.

When enabled, the extension periodically:

1. checks the local Rumble dashboard value;
2. checks the user's current live status;
3. creates a signed derived-status report; and
4. sends it to the shared backend.

#### Goal-achieved audio

The user may select a local MP3, WAV or OGG file to play when the goal is reached.

Recommended:

- clip length: 1–5 seconds;
- starting volume: 40–60%; and
- **Play once per stream/month** enabled.

The audio file stays local and is never uploaded.

![Goal achieved audio settings](https://github.com/therealtombi/RumbleChattersExtension/blob/main/docs/images/07-goal-audio-settings.pn.png)

### 7. Save and test

Click:

```text
Save & Test
```

A successful result should confirm that the configuration was saved and a signed report was accepted.

### 8. Open the Rumble earnings dashboard

Open:

```text
https://rumble.com/account/dashboard?type=earnings&interval=30
```

The extension reads the visible **Total unique chatters this month** metric from this page.

Press the refresh button in the popup after opening the dashboard for the first time.

---

## Upgrading an existing unpacked installation

Use the differential package:

```text
Rumble_Chatters_Extension_v2.2.0_Differential_Patch.zip
```

1. Back up the current extension folder.
2. Extract the differential ZIP.
3. Copy its files over the existing extension folder.
4. Do **not** uninstall the extension.
5. Open `chrome://extensions`.
6. Press **Reload** on Rumble Chatters.
7. Open Options.
8. Press **Authenticate with Streemrz**.
9. Approve the connection.
10. Press **Save & Test**.

Not uninstalling preserves the existing:

- installation ID;
- non-extractable private key;
- Rumble API URL;
- selected audio clip; and
- extension preferences.

---

# Normal operation

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
- achieved/incomplete status.

Example:

```text
42 / 75
33 more unique chatters needed
56%
```

## Live users needing chatters

Only creators who are live and below target are returned.

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

## Monthly reset behaviour

The extension resets the local monthly chatter state at:

```text
00:00 America/New_York on the first day of each month
```

This automatically handles both EST and EDT.

At the reset boundary, the extension:

1. changes the local displayed chatter count to `0`;
2. clears the previous month's maximum and cached report;
3. sends a zero-value monthly report to the backend;
4. reloads an open Rumble earnings dashboard without using its cache; and
5. rescans the dashboard shortly afterward.

The extension also checks for a missed month rollover when:

- Chrome starts;
- the extension updates;
- a scheduled report runs; or
- the dashboard provides a new metric.

A stale dashboard capture from the previous month is rejected rather than restoring the old count.

---

# Authentication and device management

## What happens during authentication

Rumble Chatters uses the Streemrz OAuth Authorization Code flow with PKCE.

The flow is:

1. The extension asks the Chatters backend to create a short-lived authentication session.
2. The backend returns a Streemrz authorization URL.
3. The user logs in and completes MFA on Streemrz.com when required.
4. The user explicitly approves Rumble Chatters.
5. Streemrz sends the authorization result directly to the Chatters backend.
6. The backend validates the Streemrz identity.
7. The browser signs a one-time claim with its local P-256 private key.
8. The backend binds that browser installation to the approved Streemrz account.

## Re-authenticate with Streemrz

Use **Re-authenticate with Streemrz** to confirm or refresh the current account binding.

The browser remains tied to the same Streemrz account unless it is explicitly disconnected first.

## Disconnect account

**Disconnect account** removes the account binding for that browser installation.

Reporting that requires Streemrz account binding will not continue until the browser is authenticated again.

## Support activation

A manual one-time activation code may be available under the Advanced/support section during migration or support work.

This is not the normal installation method. Do not publish or reuse support codes.

---

# Privacy and security

## Data that remains local

The following stays in the user's browser:

- private Rumble Livestreaming API URL;
- Rumble API key contained in that URL;
- local goal-achieved audio file;
- non-extractable P-256 private key;
- local extension preferences; and
- Rumble dashboard cookies and session data.

## Data never given to the extension

The extension never receives or stores:

- Streemrz password;
- MFA response;
- Streemrz session cookie;
- OAuth client secret;
- OAuth authorization code; or
- OAuth access token.

Authentication credentials are handled by Streemrz.com and the Chatters backend.

## Data stored by the backend

The backend stores only what is needed to operate and secure the service, including:

- stable Streemrz user ID and username for account binding;
- browser installation ID;
- Chrome extension ID and version;
- per-install credential ID;
- public key or public-key fingerprint;
- credential status and last-seen time; and
- derived Rumble reporting data.

Example derived status:

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

The backend does not store:

- private Rumble API URL;
- Rumble API key;
- Rumble password;
- Streemrz password;
- MFA code;
- dashboard cookies; or
- browser session data.

## Per-install request signing

Every installation has its own P-256 key pair.

The private key is created through WebCrypto and stored as non-extractable. The backend receives only the public identity required to validate signatures.

Signed requests include values such as:

- credential ID;
- installation ID;
- extension ID;
- timestamp;
- one-time request ID;
- HTTP method and path; and
- SHA-256 hash of the request body.

The backend rejects:

- invalid signatures;
- replayed request IDs;
- expired timestamps;
- cross-installation requests;
- disabled credentials;
- incorrect extension identities;
- stale or future reports;
- previous-month reports; and
- excessive request rates.

There is no permanent global API secret embedded in Version 2 of the extension.

---

# Troubleshooting

## Authentication was cancelled

Close the cancelled-authentication tab and press **Authenticate with Streemrz** again.

Every attempt creates a new short-lived state value. A failed or cancelled session cannot be resumed.

## Authentication reports `invalid_scope`

This is a server-side Streemrz configuration issue.

The Streemrz deployment must include the `chatters:enrol` scope and the Gunicorn service must have been restarted after applying the OAuth integration.

Required Streemrz version:

```text
p4e631 or later
```

## Clicking Authorize returns `access_denied`

This indicates an outdated OAuth consent template that submitted an empty `allow` value.

Required Streemrz version:

```text
p4e632 or later
```

## Developer text appears on the consent page

This indicates the consent-formatting fix has not been applied.

Required Streemrz version:

```text
p4e633 or later
```

## Options says authenticated but the popup list fails

Confirm:

- **Secure device connection** says **Enrolled**;
- the Streemrz account says **Authenticated**;
- **Save & Test** succeeds;
- the backend health endpoint is available; and
- the browser extension was reloaded after upgrading.

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

A future Rumble user-interface change may require a parser update.

## I am live but not shown in the shared list

Confirm:

- the Rumble API URL is present and valid;
- the URL uses HTTPS and belongs to `rumble.com`;
- scheduled reporting is enabled;
- Streemrz authentication is complete;
- the device is enrolled;
- Rumble reports the creator as live;
- the current chatter count is below the goal; and
- a recent refresh or scheduled report has completed.

A creator will not be listed after the goal is achieved.

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

# Backend and server operator notes

## Public health check

```bash
curl --fail --silent --show-error \
  https://chatters.streemrz.com/health
```

Expected minimum response:

```json
{"ok":true}
```

## Service layout

The FastAPI backend listens privately on:

```text
127.0.0.1:8011
```

Nginx proxies public HTTPS traffic to that local service.

Only these standard inbound ports should normally be exposed:

```text
22   SSH
80   HTTP
443  HTTPS
```

Do not expose port `8011` publicly.

## Useful service commands

```bash
sudo systemctl status rumble-chatters.service --no-pager --full
sudo journalctl -u rumble-chatters.service -n 100 --no-pager
sudo systemctl restart rumble-chatters.service
sudo nginx -t
sudo systemctl reload nginx
```

## Matched deployment order

For a new server deployment:

1. Deploy the Streemrz OAuth integration and consent fixes through `p4e633` or later.
2. Create the confidential Rumble Chatters OAuth application on Streemrz.
3. Register the exact callback:

   ```text
   https://chatters.streemrz.com/api/v2/streemrz-auth/callback
   ```

4. Securely configure the OAuth Client ID and Client Secret on the Chatters backend.
5. Deploy backend `0.8.0a` or later.
6. Validate the database account-binding columns and index.
7. Merge the dedicated OAuth callback location into the existing Certbot-managed Nginx configuration.
8. Validate Nginx with `sudo nginx -t` before reloading.
9. Install or upgrade extension `2.2.0`.
10. Complete a real browser authentication and signed-report test.

## Migration gates

During migration from older installations:

```env
ALLOW_LEGACY_V1_AUTH=true
REQUIRE_STREEMRZ_ACCOUNT_BINDING=false
```

After every active installation has upgraded and authenticated:

```env
ALLOW_LEGACY_V1_AUTH=false
REQUIRE_STREEMRZ_ACCOUNT_BINDING=true
```

Do not enforce account binding until active installations have completed the new authentication flow.

---

# Current application rules

```text
Goal source:
  Rumble dashboard metric

Live-status source:
  User's local Rumble Livestreaming API URL

Normal access requirement:
  Authenticated Streemrz account
  Enrolled browser installation

Backend stores:
  Account binding and device security metadata
  Derived public Rumble status

Backend does not store:
  Private Rumble API URL or key
  Passwords, MFA responses or browser session cookies

List visibility:
  Live creators below goal only

List sorting:
  Lowest percentage progress first

Achievement behaviour:
  Play local audio once per stream/month
  Remove achieved creator from the shared list

Monthly rollover:
  First day of month at 00:00 America/New_York
  Reset local count and rescan
```

---

# Version history

## Version 2.2.0

- Added Streemrz account authentication.
- Added explicit OAuth consent and existing-session reuse.
- Bound browser credentials to a Streemrz user account.
- Added account status, re-authentication and disconnect controls.
- Retained support-code activation only as an advanced fallback.

## Version 2.1.0

- Removed the permanent shared extension API secret.
- Added non-extractable P-256 device keys.
- Added per-install credentials, signed requests and replay protection.
- Added one-time enrolment codes for the initial security migration.

## Version 2

- Added first-of-month New York reset and rescan.
- Added prominent **Open Dashboard** and **Open Options** buttons.
- Added **Version 2** footer and Streemrz link.
- Added local goal-achieved audio controls.
- Improved popup layout, live-list sorting and avatar fallbacks.

---

## Streemrz

Project and service information:

```text
https://streemrz.com
```

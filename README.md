# Rumble Creator Metrics Helper 4.0.0

Rumble Creator Metrics Helper helps Rumble creators monitor their monthly Creator Program goals and find live creators who still need support.

It tracks:

- **Unique Chatters** — target: **75**
- **Watch Hours** — target: **500**

Version **4.0.0** is available as:

- **Chrome Extension 4.0.0**
- **Desktop 4.0.0 for Windows**
- **Desktop 4.0.0 for Linux**

## What you need

Before using either version, you need:

- a Rumble account;
- access to your Rumble Creator Program dashboard;
- your private Rumble Livestreaming API URL;
- a Streemrz.com account; and
- an internet connection.

# Chrome Extension 4.0.0

Use the Chrome Extension if you normally keep Chrome or a compatible Chromium browser running.

The browser must remain running for scheduled monitoring.

## Install

1. Extract:

   ```text
   Rumble_Creator_Metrics_Helper_Extension_v4.0.0.zip
   ```

   to a permanent folder.

2. Open:

   ```text
   chrome://extensions
   ```

3. Enable **Developer mode**.
4. Press **Load unpacked**.
5. Select the extracted folder containing `manifest.json`.
6. Open **Rumble Creator Metrics Helper**.
7. Press **Open Options**.
8. Authenticate with Streemrz.
9. Enter your private Rumble Livestreaming API URL.
10. Press **Save & Test**.

## Upgrade an existing Rumble Chatters extension

If you already use Rumble Chatters 3.0.1 or 4.0.0, copy the 4.0.0 upgrade files over the existing unpacked extension folder instead of uninstalling it.

Then open:

```text
chrome://extensions
```

and press **Reload** for the extension.

This keeps the existing browser installation identity and local settings.

## Extension monitoring

The extension tracks:

```text
Total unique chatters this month
Hours watched this month
```

When scheduled monitoring requires a dashboard capture, the extension uses your authenticated Rumble browser session and closes the temporary capture window automatically.

The popup shows your monthly progress and live creators who still need one or both goals.

# Desktop 4.0.0

Use Desktop if you want Rumble Creator Metrics Helper to run independently of Chrome and continue working from the Windows notification area or Linux system tray.

Desktop has its own persistent Rumble login session.

# Windows installation

Run:

```text
Rumble-Creator-Metrics-Helper-Desktop-Setup-4.0.0.exe
```

Complete the installer and launch **Rumble Creator Metrics Helper**.

No Node.js installation is required.

## Upgrade an existing Desktop installation

Quit the existing application from the tray, then run the 4.0.0 installer.

Existing Rumble sessions, Streemrz device identity and local settings are retained.

# Linux installation

## Debian / Ubuntu / Linux Mint

The recommended package is:

```text
Rumble-Creator-Metrics-Helper-Desktop-4.0.0-amd64.deb
```

Install it with:

```bash
sudo apt install ./Rumble-Creator-Metrics-Helper-Desktop-4.0.0-amd64.deb
```

Using `apt install` allows Linux to resolve the required dependencies.

If secure-storage components are installed for the first time, log out and back into the graphical desktop, or reboot, before starting the application.

## AppImage

The portable package is:

```text
Rumble-Creator-Metrics-Helper-Desktop-4.0.0-x86_64.AppImage
```

Make it executable:

```bash
chmod +x ./Rumble-Creator-Metrics-Helper-Desktop-4.0.0-x86_64.AppImage
```

Run it:

```bash
./Rumble-Creator-Metrics-Helper-Desktop-4.0.0-x86_64.AppImage
```

The AppImage requires a working secure-storage service such as GNOME Keyring / Secret Service or KWallet.

On Debian, Ubuntu or Linux Mint, if secure storage is not already available:

```bash
sudo apt update
sudo apt install -y \
  gnome-keyring \
  libpam-gnome-keyring \
  libsecret-1-0 \
  dbus-user-session \
  libappindicator3-1
```

Then log out and back in, or reboot.

# Desktop first-time setup

The Desktop application shows three connection areas:

```text
Rumble
Streemrz
Live API
```

All three must be configured before monitoring becomes active.

## 1. Sign in to Rumble

Press:

```text
Login to Rumble
```

Sign in normally and complete Rumble MFA if required.

After successful login, the application keeps its own persistent Rumble session.

## 2. Authenticate with Streemrz

Press:

```text
Authenticate
```

Complete the Streemrz login and consent process.

## 3. Configure the Rumble Live API

Open **Settings** and paste your private Rumble Livestreaming API URL into:

```text
Rumble Livestreaming API URL
```

Save the settings.

# Monitoring

When Rumble, Streemrz and the Live API are configured and monitoring is enabled, Desktop shows:

```text
MONITORING ACTIVE
```

Desktop checks live status approximately every **60 seconds**.

Creator Program metrics are refreshed approximately every **10 minutes** while monitoring is active, whether you are live or offline.

Press:

```text
Refresh now
```

to request an immediate dashboard capture.

# Monthly goals

Both clients track:

```text
Unique Chatters: 75 required
Watch Hours: 500 required
```

Watch Hours are read specifically from:

```text
Hours watched this month
```

Compact Rumble values such as `5.41K` are supported.

The current Creator Program month is checked before metrics are accepted, so previous-month values are not treated as current values.

# Live creators needing support

The support list contains creators who are currently live and still need Unique Chatters, Watch Hours or both.

Only goals that still need support are displayed.

If both goals are complete, the creator is removed from the support list.

If your own live stream qualifies, it is marked:

```text
YOU
```

# Watching another creator

Press **Watch** on a creator card to open the creator's current Rumble stream.

# Desktop settings

## Enable monitoring

Leave this enabled for normal use.

Monitoring requires Rumble login, Streemrz authentication and the Live API configuration.

## Start with system

When enabled, Desktop starts automatically when you sign in to the computer.

For AppImage installations, keep the AppImage in a permanent location before enabling this option.

## Close window to system tray

When enabled, closing the main window keeps monitoring active in the tray.

Use **Quit** from the tray menu to stop the application completely.

# Goal achieved audio

A local audio clip can be configured for goal completion.

The selected audio file remains on the local computer.

# Troubleshooting

## Creator Program values are not showing in Desktop

Press **Refresh now**.

Desktop can capture Chatters and Watch Hours while live or offline.

Press **Open Dashboard** to confirm that the current Creator Program values are visible in the authenticated Rumble session.

Before the first successful capture, Desktop displays:

```text
Awaiting dashboard capture
```

## Creator Program values are not showing in the Extension

Confirm scheduled monitoring is enabled and that the Rumble dashboard shows the current Creator Program values.

## Rumble login has expired in Desktop

Press **Login to Rumble** and sign in again.

## Streemrz authentication has expired

Authenticate with Streemrz again from the client.

## Linux secure storage is unavailable

On Debian, Ubuntu or Linux Mint:

```bash
sudo apt update
sudo apt install -y \
  gnome-keyring \
  libpam-gnome-keyring \
  libsecret-1-0 \
  dbus-user-session
```

Then log out and back in, or reboot.

KDE users can use KWallet instead.

# Privacy and security

## Rumble credentials

Desktop Rumble login takes place on Rumble's own page.

The Chrome Extension uses your existing browser Rumble session for dashboard access.

Rumble Creator Metrics Helper does not require your Rumble password to be entered into its settings.

## Streemrz credentials

Streemrz authentication is completed through Streemrz.

The application does not store your Streemrz password or MFA response.

## Rumble Livestreaming API URL

The private Rumble Livestreaming API URL remains local to the client and is not sent to the Rumble Creator Metrics Helper service backend.

## Device identity

Each installation has its own cryptographic device identity.

Authenticated reports are signed so the service can verify the registered installation and reject invalid or replayed requests.

# Service

```text
https://chatters.streemrz.com
```

Streemrz:

```text
https://streemrz.com
```

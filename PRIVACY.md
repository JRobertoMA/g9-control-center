# Privacy policy

**English** · [Español](PRIVACIDAD.md)

G9 Control contains no ads, analytics, trackers or accounts, and sends nothing on its own. It only connects to the internet in two cases, both when you ask for it: **“Send diagnostics”** and **checking for app updates**.

## Send diagnostics

The button sits in the **Controller** section and in the warning shown for controllers with experimental support. It opens a dialog with:
- a **preview of exactly what will be sent**;
- an optional contact so the developer can reply: you choose **Reddit, Telegram or email**;
- the **Send** button.

Nothing leaves the tablet until you tap **Send**.

**What is sent** (a JSON file):

| Data | Example |
|---|---|
| Controller | Model, connection mode, USB vendor/product ID and name, manufacturer, and its USB interfaces (class and endpoints) |
| Audio devices | The type and name of the audio devices Android sees (for example “USB headset”), to check the controller’s headphone jack |
| Tablet and app | Android version, tablet maker and model, G9 Control version |
| Controller configuration | Everything the app read (dead zones, lights, remaps, macros…), **without the controller’s chip ID** |
| Session log | The app’s traffic with the controller, **without the chip ID lines** |
| Contact | Only if you typed one: the type (Reddit, Telegram or email) and your user or address |

**What is not sent:** your accounts, files, location, other apps, the screen, or the controller’s unique identifier (chip ID). The server rejects any upload that contains the chip ID.

**Where it goes:**
1. The diagnostic goes to a server run by the G9 Control developer (`diag.yuutomugetsu.com`, hosted by IONOS in the EU).
2. The server forwards it to the developer through a Telegram bot.
3. Your IP address is used only to limit the number of uploads (5 per hour) and is not stored: the counter uses a one‑way hash that changes every day.

**What it is for:** adding support for more controllers and fixing problems. It is not shared with anyone else.

**How long it is kept:** **90 days**, then it is deleted automatically.

**Deleting it earlier:** the app shows an **id** after sending (for example `G9_2026-144407-454a82`). Ask the developer to delete it, quoting that id, through this repository’s *Issues*.

## App updates

In the **Controller** section, the **App updates** card checks this repository’s GitHub Releases for a newer G9 Control:
- only when you tap **Check for updates**;
- or when the app opens, if you turn on “Check when the app opens” (**off by default**), at most once a day.

The app only *reads* public release information from GitHub (`api.github.com` and its download servers). It sends nothing about you, your controller or your tablet. Like any website, GitHub sees your IP address and handles it under [its own privacy statement](https://docs.github.com/site-policy/privacy-policies/github-general-privacy-statement).

Before installing, the app checks that the file matches the published one (SHA‑256) and is signed with the same key, and Android always asks you to confirm.

## Everything else stays on your device

Backups, profiles, logs and exported diagnostics are stored in the app’s storage on your tablet. Backups include the controller’s chip ID: do not share them. Profiles and diagnostics do not include it.

**Accessibility service (optional):** “Using the tablet with the controller attached”. It only filters the G9’s keys and simulates the navigation‑bar gestures (for example for Circle to Search). It does not read the screen or other apps, records nothing and sends nothing.

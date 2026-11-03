<p align="center"><img src="assets/icon-512.png" width="128" alt="G9 Control icon"></p>

# G9 Control

**English** · [Español](README.es.md)

A lightweight, **unofficial** Android app to configure the **Lenovo Legion Gaming Controller G9 2026** (made by GameSir, USB `3537:1136`) connected over USB‑C to an Android tablet. It is an alternative to the official *Ultra Control* app that only does controller configuration: no accounts, no ads and no analytics. It only connects to the internet when you ask it to: to **send diagnostics** or to **check for app updates** ([privacy policy](PRIVACY.md)). The app is in **English and Spanish**, following the system language.

This repository contains:

- the **compiled app** (*Releases* section);
- the **documentation of the findings**: the controller’s configuration protocol, its settings and the firmware bugs found.

The source code is not published for now.

> ⚠️ Unofficial project, not related to Lenovo or GameSir. Use it at your own risk. Read the **[legal notice](DISCLAIMER.md)** before installing it.

## Download

| Version | File | SHA‑256 |
|---|---|---|
| 0.10.0‑beta.2 (test version) | `G9Control-0.10.0-beta.2.apk` (in *Releases*) | `fff9c86b3db57b5cbdc5bebca75be61c1c9e4848afd2acc709c56d635b8e6908` |
| 0.10.0‑beta.1 (test version) | `G9Control-0.10.0-beta.1.apk` (in *Releases*) | `bb52991e2a0be2f0bfdb6fcdedd78c42bb3deb9eea58d9ccd1d190c930479239` |
| 0.8.0 | `G9Control-0.8.0.apk` (in *Releases*) | `b004fc7c4bffaa020eb291a90c79106455aa28b3ce7489be17fb71e564b513df` |

Check the file before installing it: `sha256sum G9Control-0.8.0.apk`. What changed in each version: [changelog](CHANGELOG.md).

**Requirements:** Android 12 or later with USB Host (OTG), and a Legion controller connected over USB‑C (G9 2026 for full configuration). No root, Shizuku or account needed. Tested on a Lenovo Legion Y700 Gen5 (TB323FU) with controller firmware 0.10 (GCM protocol 2.8).

## Supported controllers

| Controller | USB (Android mode) | Status |
|---|---|---|
| **Legion Gaming Controller G9 2026** | `3537:1136` | ✅ Verified: everything can be configured |
| Legion Gamepad G9 无极 (Wuji) | `3537:1161` | 🧪 Experimental: reading works; settings stay read‑only until a tester verifies them |
| Legion Gaming Controller G9 (2025) | `3537:1134` | 🧪 Experimental (same commands as the 2026) |
| Legion Gaming Controller G9 Pro | name only | 🧪 Experimental (adds optical buttons, not supported yet) |
| Legion Gaming Controller G7 | `3537:1129` | 🧪 Experimental (no lights, vibration or macros; L4 and R4 only) |
| Legion Gaming Controller G3 | `3537:1154` | 🔎 Detected only: the official app has no configuration commands for it |

G9 Control only sends a controller the commands that the official app declares for that model, and never touches a controller in firmware‑update (DFU) mode.

### Help add your controller

If you have one of the experimental controllers:
1. Connect it in Android mode.
2. Go to **Controller → Send diagnostics**: you will see exactly what is sent, and it never includes the chip ID.
3. Optionally, leave a Reddit, Telegram or email contact.

With that, each area is tested with you step by step before it is unlocked. Details: [privacy policy](PRIVACY.md).

## What it can configure

| Area | Features |
|---|---|
| Sticks & triggers | Inner/outer dead zones of each stick, start/end zones of each trigger, quick trigger, D‑pad diagonal lock, left stick ↔ D‑pad swap |
| Vibration & lights | Grip vibration (levels 0–4); light rings (effect, color, saturation, brightness, speed); Home button light |
| Buttons | Remap of the 4 back buttons (all at once), macros of up to 11 keys with press/hold mode and loop |
| Controller | Information, switch to Xbox mode, ABXY layout, named profiles, restore from backup, factory defaults |
| Test | Calibration steps and full‑screen live view of sticks, triggers and buttons |
| Tablet use | Optional: controller combos and an on‑screen handle to open notifications, quick settings, recents, home and back in full‑screen apps |

Every change is written to the controller, acknowledged and **read back to verify it**. The configuration is stored in the controller itself and works in Android, Xbox and DS5 modes.

**What it does not do:** firmware updates, G‑Touch mode, Bluetooth, controllers outside the Legion family (GameSir…).

## Documentation

| Document | Contents |
|---|---|
| [User guide](docs/06-user-guide.md) | How to use the app, section by section, using the tablet with the controller attached, and FAQ |
| [Privacy policy](PRIVACY.md) | What “Send diagnostics” sends, where it goes and for how long |
| [The controller and its connection](docs/01-controller-and-connection.md) | USB identity, interfaces, Android/Xbox/DS5 modes and how to switch between them |
| [GCM protocol](docs/02-gcm-protocol.md) | Transport, frame format, checksum and command catalog |
| [Settings](docs/03-settings.md) | Controller information, sticks, triggers, vibration, lights, layout and factory defaults |
| [Buttons and macros](docs/04-buttons-and-macros.md) | Remap, macros and their limits |
| [Firmware issues and limitations](docs/05-firmware-issues-and-limitations.md) | Abnormal firmware behavior and how to avoid it |

Status legend in the documents: ✅ verified on the controller · 🔍 inferred, not verified · ❌ not supported.

## License

- **Documentation:** [CC BY‑SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) — you can share and adapt it, even commercially, crediting the source and keeping the same license.
- **App (APK):** free to use and to redistribute unmodified; no modified versions or sale.

Details: [LICENSE](LICENSE.md).

[![CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-sa/4.0/)

## Notice

Lenovo and Legion are trademarks of Lenovo. GameSir is a trademark of Guangzhou Xiaoji Technology. Xbox is a trademark of Microsoft. DualSense and PlayStation are trademarks of Sony Interactive Entertainment. They are mentioned only to identify the compatible hardware. See the full [legal notice](DISCLAIMER.md).

# Changelog

**English** · [Español](CHANGELOG.es.md)

## 0.10.0‑beta.2 — 2026‑11‑02 (test version)

- **Guided test for experimental controllers** (G9 无极 and other Legion models):
  - it tests each setting step by step: writes a test value, reads it back, asks you to check it (or detects the button by itself) and puts your value back;
  - a backup is saved first, and at the end everything is compared with it;
  - areas that pass are unlocked on your tablet, and the results can be sent with one tap.
- The updater accepts version tags with typographic hyphens.

## 0.10.0‑beta.1 — 2026‑11‑02 (test version)

Includes everything listed under 0.9.0‑beta.2, which was never published as a signed release.

- **In‑app updater:** checks this repository’s GitHub Releases (button, or on app start if you enable it; off by default), downloads the APK, verifies its SHA‑256 and signature, and installs it with Android’s confirmation. Betas are optional.
- **Legion family, read‑only:** G9 (2025), G9 Pro, G7 and G3 are detected. Each one only receives the commands the official app declares for it, and sections it lacks show “Not available on this controller” (for example the G7 has no lights, vibration or macros, and only L4/R4). The G3 is detected only.
- **Safety:** a controller in firmware‑update (DFU) mode is never opened, and the hex console cannot send commands outside the connected model’s list.
- **Headphone jack** documented: it only works in Android mode, and its volume is limited by the controller.
- **Richer diagnostics:** USB interface descriptors and Android audio devices (to help support new controllers and check the headphone jack).

## 0.9.0‑beta.2 — not published (included in 0.10.0‑beta.1)

- **New look:** Material 3 Expressive with your wallpaper colors (Material You), always dark. Side menu like the official app: Buttons, Sticks, Triggers, Vibration, Lights, Controller, Test and Advanced.
- **Dead zones as a two‑thumb slider** (inner–outer, start–end), as in the official app.
- Controller drawing with what each back button is assigned to; light effects as connected buttons.
- **Restore backup** in a floating button in each section. *Watch for changes* and *Export JSON backup* move to **Advanced**, with the raw data and the hex console.
- **Circle to Search** with View + R3 or from the handle (simulated navigation‑bar gesture; gesture navigation only).
- **Send diagnostics:** from the app, with a preview of exactly what is sent, an optional contact (Reddit, Telegram or email) and no chip ID. It is the only use of the internet, and only when you tap Send ([privacy policy](PRIVACY.md)).
- **Home as a back‑button target:** the back button sends the controller’s Guide/Home key right away, so a launcher can bind it (e.g. DroidDeck’s Quick Access Menu).
- **Live test in full screen:** the controller no longer moves the app’s focus while testing. Hold **Home** for about 2 seconds to exit (or use the Back gesture).
- **Using the tablet with the controller attached:** an optional accessibility service with controller combos (hold View + Y/X/LB/A/B/RB: notifications, quick settings, recents, home, back, handle) and an on‑screen handle like the stylus one. It only acts on the G9’s keys and does not read the screen.

## 0.8.0 — 2026‑11‑01

- **Bilingual app:** English and Spanish, following the system language. On Android 13+ the language can be chosen for this app only (*Settings → Apps → G9 Control → Language*).
- **New icon** (adaptive, with a monochrome version for themed icons).
- Labels are translated when shown, so a language change applies without restarting the app.
- **Privacy:** profiles no longer include the controller’s chip ID, so they can be shared without it. Backups still keep it.
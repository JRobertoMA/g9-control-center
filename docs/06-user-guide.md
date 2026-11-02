# G9 Control user guide

**English** · [Español](es/06-guia-de-uso.md)

G9 Control configures the **Legion Gaming Controller G9 2026** connected over USB‑C to an Android tablet (tested on the Lenovo Legion Y700 Gen5). It needs no root, account or internet connection. All settings are stored in the controller itself and work in its three modes: Android, Xbox and DS5.

The app is in **English and Spanish** and follows the system language. On Android 13+ you can pick the language for this app only: *Settings → Apps → G9 Control → Language*.

## Getting started

1. Download `G9Control-<version>.apk` from *Releases*, check its SHA‑256 and install it (Android 12 or later; Android will ask you to allow installing apps from unknown sources).
2. Connect the controller over USB‑C in **Android mode** (dark blue LED).
3. Android asks whether to open G9 Control: accept and, in the USB access prompt, tap **Allow**. Tapping outside the prompt counts as denying: tap **Connect** to bring it back.
4. On connection, the app **reads the whole configuration** (without changing anything) and every tab shows the current values.

> Tip: before changing anything, tap **Export JSON backup**. “Restore” always uses the latest backup.

Every change is written to the controller, acknowledged and **read back to verify it**. The result appears on the status line: “done and verified”.

## Tabs

### Sticks & triggers
- **Inner zone** of each stick: travel ignored around the center. **Outer zone**: point from which the stick counts as fully pushed (100 = no clipping).
- **Start and end zone** of each trigger.
- **Quick trigger** for LT and RT (requires the trigger’s back switch on long travel).
- **D‑pad:** diagonal lock and left stick ↔ D‑pad swap.

### Vibration & lights
- **Vibration** of each grip in levels 0–4.
- **Light rings** around the sticks: effect (solid, breathing, rainbow, off), color, saturation, brightness and speed, together or separately.
- **Home button light.**

### Buttons
- **Back buttons** (L4, R4, L5, R5): assign any button or stick direction to each one. From the app all four assignments fit at once; with the controller shortcut (M + button), only one.
- **Macros:** up to 11 keys in a row on a back button, each with its pause (after the previous key is released) and the time it stays pressed.
  - **Press:** each press runs the whole macro.
  - **Hold:** the macro advances only while you hold the button.
  - **Loop:** repeats the macro. With “Press”, one tap starts it and another stops it. The actual pause between loops is about half the value entered; the app shows it next to the field.
- ⚠️ **A back button cannot press two keys at once** (for example L3 + R3 for the RetroArch menu). Assign one key to each back button (L4 → L3, R4 → R3) and press them together.

### Controller
- **Information:** firmware, protocol, manufacturing date and controller identifier.
- **Connection mode:** “Switch to Xbox mode”. In Xbox the app can neither configure the controller nor switch it back to Android. To return, hold **View + Menu for 2 s** twice (Xbox → DS5 → Android), or once and then tap **Switch to Android mode** in the app. DS5 is reached from Xbox with one View + Menu.
- **Button layout:** standard or swapped (A↔B, X↔Y).
- **Profiles:** save the current configuration with a name and apply it whenever you want. Profiles can be exported to a file and imported (for example, to share them).
- **Restore:** everything from the latest backup, or **factory defaults**: sticks 10–100, triggers 5–95, vibration level 3, cyan rings, white Home, standard layout and back buttons unassigned.

### Test
- **Calibration** with the controller buttons (the app cannot calibrate over USB):
  1. Trigger switches on long travel.
  2. Hold **View + Menu + Home** until the Home LED blinks.
  3. Rotate each stick fully 2–3 times.
  4. Press each trigger fully 3 times.
  5. Press **A**.
- **Live test:** sticks (with their dead zones drawn), triggers and buttons as Android receives them. On this tab the controller buttons do not act on the app.

### Raw data
All the values read from the controller, with the raw frames. Useful for checking or for reporting a problem.

## FAQ

**When switching to Xbox, Android offers to open “Mobile Assistant”.** It is a Lenovo app offered for any USB device. Choose G9 Control and tick “Always open” so it opens directly.

**When switching modes, the Home LED does not blink green, white or blue.** The mode color blink comes out of the Home light: if you lowered its saturation and brightness, it stays off. The mode still changes; raise the Home saturation and brightness (**Vibration & lights** tab) if you want to see the blink.

**The notification “USB power supply to the connected device” appears.** That is normal: the tablet powers the controller. Do not change the “USB Preferences” options.

**I used the official app (Ultra Control) and my configuration changed.** The official app’s “Restore default” returns the controller to factory defaults. Restore your backup or apply a profile from the **Controller** tab.

**12‑key macros created with the official app.** They can hang and reboot the controller ([firmware issue](05-firmware-issues-and-limitations.md#macros-with-more-than-11-events)). G9 Control detects it and stops reading macros until you reconnect the controller. Delete it and save it again with 11 keys at most.

**Updating the firmware.** Not available in G9 Control: use the official app.

## Advanced

- **Show console and log:** shows the hex console (to send GCM commands by hand) and the log of all traffic with the controller. The log is also saved in `Android/data/dev.g9control/files/registro/`.
- Backups: `Android/data/dev.g9control/files/respaldos/`. Profiles: `…/files/perfiles/`.

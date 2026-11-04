# G9 Control user guide

**English** · [Español](es/06-guia-de-uso.md)

G9 Control configures the **Legion Gaming Controller G9 2026 and G9 (2025)** connected over USB‑C to an Android tablet (tested on the Lenovo Legion Y700 Gen5). It needs no root or account, and only uses the internet if you tap **Send diagnostics**. All settings are stored in the controller itself and work in its three modes: Android, Xbox and DS5.

The app is in **English and Spanish** and follows the system language. On Android 13+ you can pick the language for this app only: *Settings → Apps → G9 Control → Language*.

## Getting started

1. Download `G9Control-<version>.apk` from *Releases*, check its SHA‑256 and install it (Android 12 or later; Android will ask you to allow installing apps from unknown sources).
2. Connect the controller over USB‑C in **Android mode** (dark blue LED).
3. Android asks whether to open G9 Control: accept and, in the USB access prompt, tap **Allow**. Tapping outside the prompt counts as denying: tap **Connect** to bring it back.
4. On connection, the app **reads the whole configuration** (without changing anything) and every section of the side menu shows the current values.

> Tip: before changing anything, tap **Export JSON backup** (**Advanced** section). “Restore backup” always uses the latest backup.

Every change is written to the controller, acknowledged and **read back to verify it**. The result appears on the status line: “done and verified”.

## Side menu sections

In the settings sections, the floating **Restore backup** button (bottom right) rewrites that section’s settings with the latest backup.

### Sticks
- **Dead zone** of each stick with a two‑thumb slider: the left thumb is the **inner zone** (travel ignored around the center, 0–99) and the right one the **outer zone** (point from which the stick counts as fully pushed, 1–100; 100 = no clipping). As in the official app, the inner zone always stays below the outer one.
- **D‑pad:** diagonal lock and left stick ↔ D‑pad swap.

### Triggers
- **Start and end zone** of each trigger, with the same two‑thumb slider.
- **Quick trigger** for LT and RT (requires the trigger’s back switch on long travel).

### Vibration
- Grip intensity in levels 0–4, both at once or each one separately. The controller only stores these 5 levels (0, 25, 50, 75 and 100 %).
- **Games can only make the controller vibrate in DS5 mode:** in Android and Xbox mode, Android does not see its motors. The intensity you set also applies in DS5.
- **Test vibration:** with the controller in DS5 mode, the *Test vibration* card makes both motors (or each one) vibrate for a moment, with the saved intensity.
- **Emulators (PS2 and others):** if the vibration feels too strong, level 1 (25 %) is the softest setting that still vibrates. To go lower, use the emulator's own vibration or rumble scale.

### Lights
- **Light rings** around the sticks: effect (solid, breathing, rainbow, off), color, saturation, brightness and speed, together or separately.
- **Home button light.**

### Buttons
- Controller drawing with what each back button is assigned to.
- **Button layout:** standard or swapped (A↔B, X↔Y).
- **Back buttons** (L4, R4, L5, R5): assign any button or stick direction to each one. From the app all four assignments fit at once; with the controller shortcut (M + button), only one.
- **Macros:** up to 11 keys in a row on a back button, each with its pause (after the previous key is released) and the time it stays pressed.
  - **Press:** each press runs the whole macro.
  - **Hold:** the macro advances only while you hold the button.
  - **Loop:** repeats the macro. With “Press”, one tap starts it and another stops it. The actual pause between loops is about half the value entered; the app shows it next to the field.
- ⚠️ **A back button cannot press two keys at once** (for example L3 + R3 for the RetroArch menu). Assign one key to each back button (L4 → L3, R4 → R3) and press them together.

### Controller
- **Information:** firmware, protocol, manufacturing date and controller identifier.
- **Connection mode:** “Switch to Xbox mode”. In Xbox the app can neither configure the controller nor switch it back to Android. To return, hold **View + Menu for 2 s** twice (Xbox → DS5 → Android), or once and then tap **Switch to Android mode** in the app. DS5 is reached from Xbox with one View + Menu.
- **Profiles:** save the current configuration with a name and apply it whenever you want. Profiles can be exported to a file and imported (for example, to share them).
- **Restore:** everything from the latest backup, or **factory defaults**: sticks 10–100, triggers 5–95, vibration level 3, cyan rings, white Home, standard layout and back buttons unassigned.
- **Using the tablet with the controller attached:** controller combos and an on‑screen handle to open notifications, quick settings, recents, home and back without swiping from the edge (see below).

### Test
- **Calibration** with the controller buttons (the app cannot calibrate over USB):
  1. Trigger switches on long travel.
  2. Hold **View + Menu + Home** until the Home LED blinks.
  3. Rotate each stick fully 2–3 times.
  4. Press each trigger fully 3 times.
  5. Press **A**.
- **Start live test:** opens in full screen the sticks (with their dead zones drawn), triggers and buttons as Android receives them. While it is open, the controller does not act on the app. To exit, **hold Home for about 2 seconds** (or use the Back gesture).

### Advanced
- **Watch for changes:** re‑reads the controller every few seconds and highlights what changed (useful to see what a controller shortcut changes).
- **Export JSON backup.**
- **Raw data:** all the values read from the controller, with the raw frames. Useful for checking or for reporting a problem.
- **Console and log:** hex console to send GCM commands by hand and log of all traffic with the controller.

## Using the tablet with the controller attached

In full‑screen apps, the controller’s frame makes it hard to swipe from the screen edge to show the system bars. G9 Control includes its own accessibility service that solves this:

| Hold View and press | Action |
|---|---|
| Y | Notifications |
| X | Quick settings |
| LB | Recents |
| A | Home |
| B | Back |
| RB | Show or hide the handle |
| R3 | Circle to Search |

- **Circle to Search:** View + R3 or the handle simulate the navigation‑bar long press (in full‑screen apps they first reveal the bar). Requires gesture navigation.
- **On‑screen handle:** a half circle on the screen edge (like the stylus one); tapping it opens those actions, and it can be dragged along the edge. It only appears while the controller is connected.
- **Turning it on:** **Controller** section → “Using the tablet with the controller attached” card → **Enable in Accessibility**. If Android says the setting is restricted (the app does not come from a store), open **App info** → ⋮ → “Allow restricted settings” and try again.
- **Privacy:** the service only looks at the G9’s buttons; it does not read the screen, store anything or use the internet; it only simulates the navigation‑bar gestures.

## Guided test (experimental controllers)

If your controller shows the red “experimental support” band, tap **Guided test** in it. With the controller connected in Android mode and its configuration read, the test:

1. Saves a backup.
2. Goes through each setting, one step at a time:
   - it writes a test value and reads it back;
   - when needed, it asks you to check it (for example, are the rings red?) or to press a button, which it detects by itself;
   - it puts your value back.
3. At the end, reads everything again and compares it with the backup.
4. Unlocks on your tablet the areas that passed, so you can then change those settings normally.
5. Lets you **send the results**.

**Stop and restore** puts everything back at any moment. The mode‑switch step is optional: to return from Xbox mode, hold View + Menu for 2 s twice.

## Updating the app

**Controller** section → **App updates**:
- **Check for updates** looks for a newer version in this repository’s *Releases*;
- “Include test versions (beta)” also offers betas;
- “Check when the app opens” (off by default) does it automatically, once a day at most.

**Download and install** verifies the file (SHA‑256 and signature) and opens Android’s confirmation. The first time, Android asks you to allow G9 Control to install apps. Your settings, profiles and backups are kept.

The first version with the updater (0.10.0‑beta.1) has to be installed by hand.

## Sending diagnostics

**Controller** section → **Send diagnostics** (also in the warning for controllers with experimental support):
- shows exactly what will be sent and lets you add a contact (Reddit, Telegram or email) so the developer can reply;
- nothing is sent until you tap **Send**, and it never includes the controller’s chip ID;
- after sending, the app shows an id;
- **Export** saves the same file to share it yourself.

Details: [privacy policy](../PRIVACY.md).

## FAQ

**When switching to Xbox, Android offers to open “Mobile Assistant”.** It is a Lenovo app offered for any USB device. Choose G9 Control and tick “Always open” so it opens directly.

**When switching modes, the Home LED does not blink green, white or blue.** The mode color blink comes out of the Home light: if you lowered its saturation and brightness, it stays off. The mode still changes; raise the Home saturation and brightness (**Lights** section) if you want to see the blink.

**The notification “USB power supply to the connected device” appears.** That is normal: the tablet powers the controller. Do not change the “USB Preferences” options.

**I used the official app (Ultra Control) and my configuration changed.** The official app’s “Restore default” returns the controller to factory defaults. Restore your backup or apply a profile from the **Controller** section.

**12‑key macros created with the official app.** They can hang and reboot the controller ([firmware issue](05-firmware-issues-and-limitations.md#macros-with-more-than-11-events)). G9 Control detects it and stops reading macros until you reconnect the controller. Delete it and save it again with 11 keys at most.

**Updating the firmware.** Not available in G9 Control: use the official app.

## Files

- The traffic log is also saved in `Android/data/dev.g9control/files/registro/`.
- Backups: `Android/data/dev.g9control/files/respaldos/`. Profiles: `…/files/perfiles/`.

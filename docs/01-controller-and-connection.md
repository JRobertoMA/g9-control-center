# The controller and its connection

**English** · [Español](es/01-mando-y-conexion.md)

## Identification

| Field | Value |
|---|---|
| Product | Lenovo Legion Gaming Controller G9 2026 (model LGC910) |
| Actual manufacturer | GameSir (Guangzhou Xiaoji Technology) |
| VID:PID (Android mode) | `3537:1136` |
| USB name | `Legion Gaming Controller G9 2026` |
| Input device name | `GameSir Legion Gaming Controller G9 2026` (+ `… Consumer Control`) |
| Power | Over USB‑C from the tablet (5 V, 0.5 A), no battery |
| Tested firmware | 0.10 · GCM protocol 2.8 |
| Update mode (DFU) | PID `2003`, name `…G9 2026_DFU` 🔍 (not used) |

## USB interfaces in Android mode

| Interface | Class | Endpoint | Direction | Use |
|---|---|---|---|---|
| 0 | HID | `0x81` | IN | Gamepad input |
| 0 | HID | `0x01` | OUT | Gamepad output (system rumble) |
| **1** | HID | **`0x83`** | **IN** | **Configuration responses (GCM)** |
| **1** | HID | **`0x03`** | **OUT** | **Configuration commands (GCM)** |

All endpoints are 64‑byte *interrupt* endpoints.

- Configuration goes through **interface 1**. GCM frames are written **as is**, with no report ID and no padding to 64 bytes.
- On Android the standard USB Host API is enough (`claimInterface` + `bulkTransfer`, which also works on *interrupt* endpoints) plus the normal USB permission. No root needed.
- While interface 1 is claimed, the gamepad (interface 0) keeps working normally.

### With headphones plugged in

Plugging something into the 3.5 mm jack makes the controller **disconnect and re‑enumerate** with three USB Audio interfaces in front of the two HID ones:
- audio control;
- 48 kHz 16‑bit stereo output on `0x04`;
- 48 kHz 16‑bit mono microphone input on `0x84`.

The GCM channel becomes interface 4. Unplugging goes back to the two HID interfaces. See [audio](05-firmware-issues-and-limitations.md#headphone-jack).

## Connection modes

The controller has three modes. When it changes mode it **disconnects and reconnects** with a different USB identity:

| Mode | LED | VID:PID | USB identity | GCM configuration |
|---|---|---|---|---|
| **Android** | Dark blue | `3537:1136` | GameSir / Legion Gaming Controller G9 2026 | ✅ Yes |
| **Xbox** | Green | `045E:028E` | Manufacturer “2.4G” / Xbox 360 Controller (XInput) | ❌ No |
| **DS5** | White | `054C:0CE6` | Sony Interactive Entertainment / Legion Gaming Controller G9 2026 | ❌ No |

- Each mode’s LED color is shown on the **Home LED**. If the Home light has low saturation and brightness, **the color blink is not visible** when switching modes: the LED stays off, although the mode still changes ([details](03-settings.md#home-light-05-08)).
- **The configuration can only be read and written in Android mode**, but it is stored in the controller and **applies in all three modes**. For example, a dead zone of 50 written in Android works the same in Xbox and DS5.
- The G‑Touch mode (mapping to on‑screen touches) mentioned by the manufacturer has not been studied.

### Switching modes

| From | To | How |
|---|---|---|
| Any | Next | Hold **View + Menu for 2 s**. Cycle: Android → Xbox → DS5 → Android |
| Android | Xbox | Command `08 02 07 01 08 01` ✅ (see below) |
| DS5 | Android | 48‑byte HID output report on interface 0: `02 00 04 00 … 00 FF FF FF` ✅ |
| Xbox | — | No configuration channel: buttons only |

**Mode switch by command** (`08 02 07 01 <main mode> <submode> sum8`):

| Request | Result |
|---|---|
| `08 02 07 01 08 01 1B` (mode 8, “DS5” on other GameSir controllers) | The controller switches to **Xbox**, not DS5 ✅ |
| `08 02 07 01 0B 01 1E` (mode 11, “X‑INPUT”) | Rejected: replies `08 02 07 01 01 01 14` and stays in Android ✅ |
| `08 02 07 01 01 01 14` (mode 1, submode 1) | No mode change; resets the submode ✅ |

There is no known way to go from Android straight to DS5: switch to Xbox and press View + Menu.

### Reading the mode (`08 02`)

`08 02 05 00 0F` → `08 02 07 00 <main> <submode> sum8`. In Android the main mode reads `01` (HID+GCM).

| Submode | Name | Notes |
|---|---|---|
| `01` | PROTOZOA | Factory value |
| `02` | GCM | — |
| `03` | GAME_HUB | **Written by the official app when it connects.** In this submode the controller also sends its input state over interface 1 (64‑byte packets starting with `43`). Going back to `01` with `08 02 07 01 01 01` stops it |

## Tablet behavior (Lenovo Legion Y700 Gen5)

- **When switching to Xbox**, Android offers to open “Mobile Assistant” (Lenovo/Motorola), which registers for **any** USB device. G9 Control registers for the G9 in Xbox mode (`045E:028E` + manufacturer “2.4G”) and in DS5 mode (`054C:0CE6` + G9 name), so you can pick it and tick “Always open”.
- **“USB power supply to the connected device”:** this is a normal Android notification and appears on every reconnection. Do not change its “USB Preferences”: changing the port role can break communication with the controller.

## Controller shortcuts (checked against the protocol)

| Function | Shortcut | Affected setting |
|---|---|---|
| Assign a back button | M + back button → target button → back button | Remap (the shortcut keeps only **one** assignment) |
| Quick trigger | M + LT/RT for 2 s (switch on long travel) | Quick trigger |
| Swap A↔B, X↔Y | M + A/B/X/Y for 2 s | Layout |
| Ring effect | M + left stick ← → | Effect |
| Ring brightness / saturation / color | M + left stick ↑↓ / M + D‑pad ↑↓ / M + rotate the stick | HSB color |
| Vibration | M + View (−) / M + Menu (+) | Both grips, steps of 25 |
| Turbo | M + button (M + Home for 2 s clears all) | Cannot be read or written over GCM |
| Calibration | View + Menu + Home → move sticks and triggers → A | Buttons only |
| Mode switch | View + Menu for 2 s | See above |

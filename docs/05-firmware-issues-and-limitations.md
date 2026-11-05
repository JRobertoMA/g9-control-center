# Firmware issues and limitations

**English** · [Español](es/05-fallos-y-limitaciones.md)

Abnormal behavior found with firmware 0.10 (GCM 2.8). G9 Control takes it into account; any other tool that talks GCM to this controller should too.

## Macros with more than 11 events

The controller accepts 12 events split into two packages (`01` with 11 and `FF` with the last one), but:

1. **The last package’s acknowledgment is broken:** `53 FF 0A 03 00 32`, which are the first bytes of the request, with no valid checksum.
2. **The controller hangs ⚠️.** After reading a multi‑package macro, the next event read (`53 FE`), for the same button or another one, gets no response, and **about 16 seconds later the controller reboots** (it disconnects and reconnects). Reading the macro to the end does not prevent it. Only rebooting the controller fixes it (unplug and plug it back in). Other queries keep working in the meantime.
3. A write acknowledged right before the hang **may not be saved**.

In testing, no configuration was lost in any of the reboots.

**Reading several packages:** requesting package n returns package n+1 (`53 FE … 00` → package `01`; `53 FE … 01` → package `FF`).

**Recommendation:** use macros of **11 events at most** (a single package, index `00`). If a multi‑package macro already exists (for example, created with the official app), read it once and do not send `53 FE` again until the controller is reconnected. Then delete it with `53 FC`.

## Checksum +1 in the remap of a macro button

Reading the remap of a button that runs a macro returns `1F 00 <button> FF FF <sum8 + 1>`. For example, `1F 00 03 FF FF 21` when the sum is `20`. If a correct checksum is required, that response is discarded and the read fails. It has to be accepted as a special case.

## Ring color is rounded

When storing a color, the controller rounds it down (H 179 → 178, B 25 → 24), and every rewrite of the read value darkens it a bit more. To get an exact value, write the target plus the observed difference. Verify with a tolerance of ±2.

## Ring byte 10

The controller ignores writes to that byte (the official app calls it “brightness”). Only the factory reset changes it (to 128). The actual brightness is the color’s B.

## The “DS5” mode value leads to Xbox

`08 02 07 01 08 01`, the value other GameSir controllers use for DS5, puts the G9 2026 into **Xbox mode**. Value 11 (X‑INPUT) is rejected. There is no known way to go from Android straight to DS5. In Xbox there is no configuration channel: going back requires View + Menu.

## GAME_HUB submode

The official app sets submode `03` (GAME_HUB) every time it connects. In that submode the controller also sends **its input state over the configuration channel** (64‑byte packets starting with `43`). Ignore them or go back to submode `01` with `08 02 07 01 01 01`.

## The mode blink depends on the Home light

The color blink that shows the mode (green Xbox, white DS5, blue Android) comes out of the Home LED. With **low Home saturation and brightness**, that blink **is not visible**: the LED stays off when switching modes, although the switch happens. Raise the Home saturation and brightness if you want to see it ([Home light](03-settings.md#home-light-05-08)).

## Headphone jack

Tested with three different headphones:

| Mode | Headphones |
|---|---|
| **Android** | ✅ Work: Android sees the controller as a USB headset (output and microphone) |
| Xbox | ❌ No sound: the controller exposes no audio; the tablet plays it |
| DS5 | ❌ No sound: the controller exposes DualSense‑style audio, but Android only creates the microphone; the tablet plays it |

**Volume is low** even with Android at maximum:
- Home + D‑pad ↑/↓ only sends normal volume keys to Android;
- the official app has no audio command;
- the USB sound card’s own volume cannot be reached without root.

G9 Control cannot raise it.

## The official app can overwrite the configuration

Using “Restore default” in the official app returns the controller to the [factory defaults](03-settings.md#factory-defaults) without further warning. If you switch between apps, save a backup or a profile first.

## Controller limitations

| Feature | Status |
|---|---|
| Two keys at once on a back button (via remap or macro) | ❌ Not supported |
| Turbo | Controller buttons only; cannot be read or written |
| Calibration | Controller buttons only (View + Menu + Home) |
| Gyroscope | None |
| Trigger vibration | Does not seem to have trigger motors |
| Trigger travel switches | Mechanical; cannot be read |
| Input type (`08 03`), turbo key binding (`56`) | Do not respond |
| Command `15` (“restore configuration”) | Unknown format; not used |
| Turning the controller off on demand | ❌ No power‑off command; the sleep‑time command (`26`) gets no response; cutting USB power needs root. It sleeps after 10 minutes idle. You can turn its lights off (rings “Off”, Home at minimum) |
| Firmware update | Out of this project’s scope |

## Known reports

- **G9 (2025), light rings:** one tester saw only the LEDs on the left side light up when the app set both rings to solid red, although the controller reported that it saved red on both. Another tester’s rings worked correctly. The cause is still unknown.
- **G9 (2025), not tested yet:** *Switch to Android mode* (from DS5) and *Factory defaults*. If *Factory defaults* reports the rings as not verified, the reset may still have worked: the app compares with the G9 2026’s factory settings. Export a backup first.
- **Tablet USB‑C ports:** only one carries data at a time. On a Y700 Gen5, with a USB drive in the high‑speed port, the controller keeps power but drops off USB, whichever was connected first. Remove the drive and the controller comes back by itself. A user reports the same on a Gen3. It looks like a limit of the tablet, and the app can’t change it.

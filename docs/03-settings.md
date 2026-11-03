# Controller settings

**English** · [Español](es/03-ajustes.md)

Every write in this document was verified on the controller: write → acknowledgment → read‑back with the expected value. Values **are stored in the controller**, survive reconnection and apply in Android, Xbox and DS5 modes.

## Controller information

### `01 01`

2‑byte request, no `len` and no checksum. 20‑byte response, for example:

```
C9 00 00 00 00 00 00 01 00 00 00 0A 00 14 17 0B 15 08 02 00
```

| Bytes | Field | Example |
|---|---|---|
| 0 | Header `C9` | — |
| 1–6 | MAC | empty (the G9 2026 has no Bluetooth) |
| 7–8 | Hardware (`b8.b7`) | 0.1 |
| 9–10 | USB version (`b10.b9`) | 0.0 |
| 11–12 | Firmware (`b12.b11`) | 0.10 |
| 13–16 | Manufacturing date | `14 17 0B 15` → 2023/11/21 |
| 17–18 | GCM version (`b18.b17`) | 2.8 |
| 19 | Battery | 0 (no battery) |

**Date:** bytes 13 and 14 are the century and the year in decimal, concatenated (`20`·`23` = 2023), not a 16‑bit integer.

### `50`

1‑byte request. 17‑byte response: `50`, date (4 bytes, same format), **chip ID** (9 bytes, unique to each controller), model ID (`16` = 22), color ID (`00`) and `sum8`.

## Dead zones (`07`)

Read: `07 <zone> 06 00 00 sum8`. Write: `07 <zone> 06 01 <value> sum8`. The controller replies with an echo.

| Zone | Setting | Range in G9 Control | Factory |
|---|---|---|---|
| `01` / `02` | Left / right stick · inner zone | 0–99 | 10 |
| `03` / `04` | Left / right stick · outer zone | 1–100 | 100 |
| `05` / `06` | LT / RT · start zone | 0–99 | 5 |
| `07` / `08` | LT / RT · end zone | 1–100 | 95 |

Values are percentages of travel. The ranges are the official app’s: inner (or start) always below outer (or end). The inner zone is applied by **rescaling**: output rises smoothly from 0, with no jump. Example: `07 01 06 01 0F 1E` sets the left stick inner zone to 15.

## Quick trigger (`21`)

Read: `21 00 01 01 23` → `21 00 <LT> <RT> sum8`. Write: `21 01 <LT> <RT> sum8`, with `01` = on and `02` = off. It requires the trigger’s physical switch on **long travel**.

## D-pad functions (`23`)

`23 <function> 06 <action> <0/1> sum8`:

| Function | Setting |
|---|---|
| `01` | D‑pad diagonal lock |
| `04` | Swap left stick ↔ D‑pad |

Example: `23 01 06 01 01 2C` turns on the diagonal lock.

## Vibration (`25 01`)

Read: `25 01 0D 00` + 8 × `00` + `33`. Write: `25 01 0D 01` + 8 bytes + `sum8` (echo).

| Byte | Field |
|---|---|
| 4 / 8 | Left / right grip intensity (0–100) |
| 5–7 / 9–11 | Trigger vibration (intensity, on press, synced). The G9 2026 stores `00`: it does not seem to have trigger motors |

The official interface uses **levels 0–4** (= 0, 25, 50, 75, 100). The M + View/Menu shortcut changes both grips in steps of 25.

## Light rings (`05 0C`)

The stick rings are “strips” 1 (left) and 2 (right).

| Operation | Frame | Response |
|---|---|---|
| Read | `05 0C 06 00 <strip> sum8` | `05 0C 0C 00 <strip> H S B <effect> <speed> <b10> sum8` |
| Write | `05 0C 0C 01 <strip> H S B <effect> <speed> <b10> sum8` | Acknowledgment `05 0C 07 01 <strip> 01 sum8` (not an echo) |
| Factory defaults | `05 0C 06 02 00 19` (both strips) | The restored values |

- **HSB color:** H 0–255 (= 0–360°), S 0–100, B 0–100. **B is the actual brightness.**
- **Effects:** `01` solid · `02` breathing · `04` rainbow · `FF` off. The M + stick ← → shortcut cycles through them in that order.
- **Speed:** 0–255 (animated effects only). The official interface shows it in %: `byte = (% × 255 + 50) / 100`.
- **Byte 10:** the official app calls it “brightness”, but **the controller ignores writes to it** and it has no visible effect. It only changes with the factory reset (which sets it to 128).
- ⚠️ **The controller rounds the color down when storing it:** writing H 179 S 100 B 25 reads back as 178 / 100 / 24. To get the exact value, write the target plus the observed difference (180 → stored as 179). When verifying, allow a tolerance of ±2.

## Home light (`05 08`)

| Operation | Frame |
|---|---|
| Read | `05 08 06 00 C2 D5` → `05 08 0A 00 C2 H S B 01 sum8` |
| Write | `05 08 0A 01 C2 H S B 01 sum8` (echo) |
| Factory defaults | `05 08 06 02 C2 D7` |

`C2` is the Home button code. Only effect: solid (`01`). The minimum brightness the controller stores is **1**: writing `05 08 0A 01 C2 00 00 00 01 DB` (B 0) reads back as B 1.

> ⚠️ **The mode color blink uses the Home LED.** If you lower the Home light’s **saturation and brightness**, the color blink for Xbox (green), DS5 (white) or Android (blue) **is not visible** when switching modes: the Home LED stays off. The mode still changes. To see the blink again, raise the Home light’s saturation and brightness. (Observed by a user with firmware 0.10.)

## Button layout (`1E`)

Read: `1E 00 1E` → `1E <value> sum8`. Write: `1E 10 2E` (standard) or `1E 04 22` (swapped: A↔B, X↔Y). The controller replies with an echo. Same as the M + A/B/X/Y shortcut held for 2 s.

## Factory defaults

The official app’s “Restore default” **does not use a reset command**: it rewrites each setting with its default value. Those values are:

| Setting | Factory value |
|---|---|
| Sticks · inner / outer zone | 10 / 100 |
| Triggers · start / end zone | 5 / 95 |
| Quick trigger, diagonal lock, left stick ↔ D‑pad | Off |
| Grip vibration | 75 (level 3) |
| Rings | H 127 (≈179°, cyan), S 100, B 50, solid, speed 128 (with `05 0C 06 02 00`) |
| Home light | H 0, S 0, B 100: white (with `05 08 06 02 C2`) |
| Layout | Standard (`1E 10`) |
| Back buttons | Unassigned, no macros (`53 FC` and `1F 02` on each one) |

G9 Control applies exactly this list and verifies every value (19 writes).

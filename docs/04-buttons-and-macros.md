# Back buttons and macros

**English** · [Español](es/04-botones-y-macros.md)

Only the four **back buttons** can be remapped or hold macros.

## Key codes

The protocol uses two code spaces:

| Space | Use | Values |
|---|---|---|
| **Button index** (1 byte) | Source button in remaps and macros | L4 = `01`, R4 = `02`, L5 = `03`, R5 = `04` |
| **HID code** (1 byte) | Target key and macro keys | Table below |

| HID | Key | HID | Key | HID | Key |
|---|---|---|---|---|---|
| `B0`–`B3` | D‑pad ↑ ↓ ← → | `BA` / `BB` | LB / RB | `C0` / `C1` | L3 / R3 |
| `B4` / `B5` | A / B | `BC` / `BD` | View / Menu | `C2` | Home (not remappable) |
| `B7` / `B8` | X / Y | `BE` / `BF` | LT / RT | `EF`–`F6` | Left stick ← ↖ ↑ ↗ → ↘ ↓ ↙ |
| | | | | `F7`–`FE` | Right stick (same directions) |

Special values: `00` = unassigned · `FF` = macro.

## Remap (`1F`)

| Operation | Frame | Response |
|---|---|---|
| Read | `1F 00 <button> 00 00 sum8` | `1F 00 <button> <target> <extra> sum8` |
| Assign | `1F 01 <button> <target HID> 00 sum8` | Exact echo |
| Unassign | `1F 02 <button> 00 00 sum8` | Exact echo. **Only affects that button** |

In the read response, byte 4 is `FF` if the button runs a macro (see [issues](05-firmware-issues-and-limitations.md#checksum-1-in-the-remap-of-a-macro-button)).

Examples checked with `getevent`:

| Request | Effect when pressing the button |
|---|---|
| `1F 01 01 C0 00 E1` (L4 → L3) | `BTN_THUMBL` |
| `1F 01 02 C1 00 E3` (R4 → R3) | `BTN_THUMBR` |
| `1F 01 03 B4 00 D7` (L5 → A) | `BTN_GAMEPAD` (A) |
| `1F 01 04 F1 00 15` (R5 → left stick ↑) | `ABS_Y` fully up and back to center |

- **Over the protocol all four assignments fit at once.** With the controller’s M + button shortcut, however, only one is kept: assigning a back button clears the others.
- ❌ **Two keys at once are not possible.** `1F 01 03 C0 C1` (L5 → L3 + R3) is acknowledged, but the controller only stores the first key. For a combination such as L3 + R3 (RetroArch menu), assign one key to each back button and press them together.

## Macros (`53`)

In macro frames, **byte 3 is the button** (`01`–`04`).

| Sub | Frame | What it does | Response |
|---|---|---|---|
| `FE` | `53 FE 06 <button> <n> sum8` | Read events | `53 <package> <len> <button> <events> sum8` |
| `FD` + `00` | `53 FD 06 <button> 00 sum8` | Read settings | `53 FD 0B <button> 00 <mode> <loop> <pause hi> <pause lo> <sens> sum8` |
| `FD` + `01` | `53 FD 0B <button> 01 <mode> <loop> <pause hi> <pause lo> 00 sum8` | Save settings. ⚠️ **Erases the events** | `53 FD 07 <button> 01 00 sum8` |
| `00` | `53 00 <len> <button> <events> sum8` | Save events (up to 11) | `53 00 06 <button> 00 sum8` |
| `FC` | `53 FC 05 <button> sum8` | **Delete the macro** | `53 FC 06 <button> 00 sum8` |

In the acknowledgments, `00` in the result position means “OK”.

### Saving a macro

1. **Settings first**, because writing them erases the stored events.
2. **Events next.**
3. There is no need to assign the macro with `1F`: when the events are saved, the controller **marks the button as a macro** by itself.

```
→ 53 FD 0B 03 01 00 01 07 D0 00 37        L5: press mode, loop, pause 0x07D0
← 53 FD 07 03 01 00 5B                    OK
→ 53 00 0F 03 00 00 B7 00 64 01 2C B8 00 64 C9
                                           X (pause 0, 100 ms) · Y (pause 300, 100 ms)
← 53 00 06 03 00 5C                       OK
```

### Events

Groups of 5 bytes after the 4‑byte header:

| Bytes | Field | Measured behavior |
|---|---|---|
| 0–1 | Pause (ms, big‑endian) | From **the release of the previous key** until this one is pressed. The first event uses 0 |
| 2 | Key (HID) | Buttons and stick directions |
| 3–4 | Duration (ms, big‑endian) | Time the key stays pressed (exact, ±4 ms) |

Events run **one after another**: a macro cannot press two keys at once. Limits: 4095 ms per pause or duration (GCM 2.8) and 11 events per package. The controller accepts 12 in two packages, but that causes failures ([see](05-firmware-issues-and-limitations.md#macros-with-more-than-11-events)).

### Settings

| Byte | Field | Values |
|---|---|---|
| 5 | Trigger mode | `00` **press** · `01` **hold** |
| 6 | Loop | `00` / `01` |
| 7–8 | Pause between loops | The actual pause is ≈ **0.53 × the value** |
| 9 | Sensitivity | Ignored by the controller: always `00` |

Measured behavior:

| Mode | No loop | With loop |
|---|---|---|
| **Press** | Each press runs the whole macro once | **Toggle:** one tap makes it repeat non‑stop until the next tap. If the button is held, it repeats while held |
| **Hold** | Advances only while the button is held | Repeats while held and stops on release |

| Pause between loops written | Measured actual pause |
|---|---|
| 500 | ~263 ms |
| 2000 | ~1050 ms |

### Deleting a macro

`53 FC` leaves the button clean with a single frame: events and settings reset and, if it was running the macro, unassigned. If the button had a key assigned, it is left untouched.

## Notice `53 FB`

The controller sends it on its own when a back button is configured with the M + button shortcut: `53 FB 06 <button> <state> sum8`.

| Moment | State |
|---|---|
| On entering configuration and on pressing the target | `00` |
| On saving | `01` |

If the app is not connected, the notice waits on the endpoint and arrives on connection. It may arrive while another response is expected: filter it out.

## Turbo

Turbo (M + button; the ring blinks red when enabled and green when disabled) **cannot be read or written over GCM**. It does not appear in any query and the candidate command (`56`) does not respond. It can only be set with the controller’s buttons.

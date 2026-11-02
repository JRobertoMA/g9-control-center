# GCM protocol

**English** · [Español](es/02-protocolo-gcm.md)

**GCM** is the configuration protocol of GameSir controllers. On the G9 2026 it runs over USB interface 1: commands on endpoint `0x03` and responses on `0x83` ([connection](01-controller-and-connection.md)).

## Standard frame

| Byte | Field | Description |
|---|---|---|
| 0 | `cmd` | Command (area) |
| 1 | `sub` | Subcommand or index |
| 2 | `len` | **Total** frame length, checksum included |
| 3 | `action` | `00` read · `01` write · `02` reset (depending on the command) |
| 4 … n−2 | data | Parameters |
| n−1 | `sum8` | Checksum |

**Checksum:** sum of all previous bytes, truncated to 8 bits.

```
07 01 06 00 00 0E        read the left stick inner dead zone
│  │  │  │  │  └─ sum8 = 07+01+06+00+00 = 0E
│  │  │  │  └──── data (empty when reading)
│  │  │  └─────── action 00 = read
│  │  └────────── len 6
│  └───────────── sub 01
└──────────────── cmd 07 (dead zones)
```

## Variants

| Command | Shape |
|---|---|
| `01 01` information | 2 bytes, no `len` and no checksum. Replies with 20 bytes starting with `C9` |
| `50` identifier | 1 byte, no checksum |
| `21` quick trigger | `21 <sub> <LT> <RT> sum8` (no `len`) |
| `1E` layout | `1E <sub> sum8` |
| `1F` remap | `1F <operation> <button> <target> <extra> sum8` (no `len`) |
| `53` macros | `53 <sub> <len> <button> … sum8`. Byte 3 is the button, not the action |

## Request and response

- The controller replies on `0x83`. The response starts with the same `cmd` as the request, except the information response, which starts with `C9`.
- **Writes are confirmed with an echo** of the frame (or a short acknowledgment: rings and macros). The reliable way to verify a write is to **read the setting back**.
- The controller may send **unsolicited packets**: the `53 FB` notice when a button is configured with the shortcut, and the `43 …` input state in the GAME_HUB submode. Only accept as a response packets with the expected `cmd` and a correct checksum.
- With a timeout of about 400 ms and 3 retries, the 31 reads of the full configuration take under a second.

## G9 2026 commands

The controller only responds to a specific list of commands. Those not on it, such as `08 03` (input type) or `56` (key binding with turbo), **do not respond**.

| Cmd | Sub | Action | Function | Status | Details |
|---|---|---|---|---|---|
| `01` | `01` | — | Information (firmware, GCM, manufacturing date) | ✅ | [Settings](03-settings.md#controller-information) |
| `50` | — | — | Identifier (date, chip ID, model, color) | ✅ | [Settings](03-settings.md#controller-information) |
| `08` | `02` | `00`/`01` | Main mode and submode | ✅ | [Connection](01-controller-and-connection.md#switching-modes) |
| `07` | `01`–`08` | `00`/`01` | Stick and trigger dead zones | ✅ | [Settings](03-settings.md#dead-zones-07) |
| `21` | `00`/`01` | — | Quick trigger | ✅ | [Settings](03-settings.md#quick-trigger-21) |
| `23` | `01`, `04` | `00`/`01` | Diagonal lock; left stick ↔ D‑pad | ✅ | [Settings](03-settings.md#d-pad-functions-23) |
| `25` | `01` | `00`/`01` | Vibration | ✅ | [Settings](03-settings.md#vibration-25-01) |
| `05` | `0C` | `00`/`01`/`02` | Light rings (`02` = factory defaults) | ✅ | [Settings](03-settings.md#light-rings-05-0c) |
| `05` | `08` | `00`/`01`/`02` | Home light (`02` = factory defaults) | ✅ | [Settings](03-settings.md#home-light-05-08) |
| `1E` | `00`, `04`, `10` | — | Button layout | ✅ | [Settings](03-settings.md#button-layout-1e) |
| `1F` | `00`/`01`/`02` | — | Back button remap | ✅ | [Buttons](04-buttons-and-macros.md#remap-1f) |
| `53` | `FE`, `FD`, `FC`, `00`… | — | Macros | ✅ | [Buttons](04-buttons-and-macros.md#macros-53) |
| `53` | `FB` | — | Spontaneous notice when a button is configured with the shortcut | ✅ | [Buttons](04-buttons-and-macros.md#notice-53-fb) |
| `15` | `01`/`02` | — | “Restore configuration”: unknown format | ❓ not used | — |

> ⚠️ Some commands **look like queries but are not**: `53 FC` deletes a macro, `1F 02` unassigns a button and `53 FD … 02` resets a macro’s settings.

> ⛔ Do not send firmware update commands or anything that puts the controller into DFU mode. They are not documented here.

# Botones traseros y macros

[English](../04-buttons-and-macros.md) · **Español**

Solo los cuatro **botones traseros** se pueden remapear o tener macros.

## Códigos de tecla

El protocolo usa dos espacios de códigos:

| Espacio | Uso | Valores |
|---|---|---|
| **Índice de botón** (1 byte) | Botón de origen en remapeos y macros | L4 = `01`, R4 = `02`, L5 = `03`, R5 = `04` |
| **Código HID** (1 byte) | Tecla destino y teclas de una macro | Tabla siguiente |

| HID | Tecla | HID | Tecla | HID | Tecla |
|---|---|---|---|---|---|
| `B0`–`B3` | Cruceta ↑ ↓ ← → | `BA` / `BB` | LB / RB | `C0` / `C1` | L3 / R3 |
| `B4` / `B5` | A / B | `BC` / `BD` | Vista / Menú | `C2` | Home (no remapeable) |
| `B7` / `B8` | X / Y | `BE` / `BF` | LT / RT | `EF`–`F6` | Stick izq. ← ↖ ↑ ↗ → ↘ ↓ ↙ |
| | | | | `F7`–`FE` | Stick der. (mismas direcciones) |

Especiales: `00` = sin asignar · `FF` = macro.

## Remapeo (`1F`)

| Operación | Trama | Respuesta |
|---|---|---|
| Leer | `1F 00 <botón> 00 00 sum8` | `1F 00 <botón> <destino> <extra> sum8` |
| Asignar | `1F 01 <botón> <HID destino> 00 sum8` | Eco exacto |
| Dejar sin asignar | `1F 02 <botón> 00 00 sum8` | Eco exacto. **Solo afecta a ese botón** |

En la respuesta de lectura, el byte 4 vale `FF` si el botón ejecuta una macro (ver [fallos](05-fallos-y-limitaciones.md#checksum-1-en-el-remapeo-de-un-botón-con-macro)).

Ejemplos comprobados con `getevent`:

| Petición | Efecto al pulsar el botón |
|---|---|
| `1F 01 01 C0 00 E1` (L4 → L3) | `BTN_THUMBL` |
| `1F 01 02 C1 00 E3` (R4 → R3) | `BTN_THUMBR` |
| `1F 01 03 B4 00 D7` (L5 → A) | `BTN_GAMEPAD` (A) |
| `1F 01 04 F1 00 15` (R5 → stick izq. ↑) | `ABS_Y` a tope arriba y vuelta al centro |

- **Desde el protocolo caben las cuatro asignaciones a la vez.** Con el atajo M + botón del mando, en cambio, solo se conserva una: asignar un botón trasero borra la asignación de los demás.
- ❌ **Dos teclas a la vez no son posibles.** `1F 01 03 C0 C1` (L5 → L3 + R3) se acusa, pero el mando solo guarda la primera tecla. Para una combinación como L3 + R3 (menú de RetroArch), asigna una tecla a cada botón trasero y púlsalos juntos.

## Macros (`53`)

En las tramas de macro, el **byte 3 es el botón** (`01`–`04`).

| Sub | Trama | Qué hace | Respuesta |
|---|---|---|---|
| `FE` | `53 FE 06 <botón> <n> sum8` | Leer eventos | `53 <paquete> <len> <botón> <eventos> sum8` |
| `FD` + `00` | `53 FD 06 <botón> 00 sum8` | Leer ajustes | `53 FD 0B <botón> 00 <modo> <bucle> <pausa hi> <pausa lo> <sens> sum8` |
| `FD` + `01` | `53 FD 0B <botón> 01 <modo> <bucle> <pausa hi> <pausa lo> 00 sum8` | Guardar ajustes. ⚠️ **Borra los eventos** | `53 FD 07 <botón> 01 00 sum8` |
| `00` | `53 00 <len> <botón> <eventos> sum8` | Guardar eventos (hasta 11) | `53 00 06 <botón> 00 sum8` |
| `FC` | `53 FC 05 <botón> sum8` | **Borrar la macro** | `53 FC 06 <botón> 00 sum8` |

En los acuses, `00` en la posición del resultado significa “correcto”.

### Guardar una macro

1. **Ajustes primero**, porque escribirlos borra los eventos guardados.
2. **Eventos después.**
3. No hace falta asignar la macro con `1F`: al guardar los eventos, el mando **marca el botón como macro** él solo.

```
→ 53 FD 0B 03 01 00 01 07 D0 00 37        L5: modo pulsar, bucle, pausa 0x07D0
← 53 FD 07 03 01 00 5B                    correcto
→ 53 00 0F 03 00 00 B7 00 64 01 2C B8 00 64 C9
                                           X (pausa 0, 100 ms) · Y (pausa 300, 100 ms)
← 53 00 06 03 00 5C                       correcto
```

### Eventos

Grupos de 5 bytes detrás de la cabecera de 4:

| Bytes | Campo | Comportamiento medido |
|---|---|---|
| 0–1 | Pausa (ms, big‑endian) | Desde que **se soltó la tecla anterior** hasta pulsar esta. El primer evento lleva 0 |
| 2 | Tecla (HID) | Botones y direcciones de stick |
| 3–4 | Duración (ms, big‑endian) | Tiempo que la tecla queda pulsada (exacto, ±4 ms) |

Los eventos van **uno detrás de otro**: una macro no puede pulsar dos teclas a la vez. Límites: 4095 ms por pausa o duración (GCM 2.8) y 11 eventos por paquete. El mando admite 12 en dos paquetes, pero eso provoca fallos ([ver](05-fallos-y-limitaciones.md#macros-de-más-de-11-eventos)).

### Ajustes

| Byte | Campo | Valores |
|---|---|---|
| 5 | Modo de disparo | `00` **pulsar** · `01` **mantener** |
| 6 | Bucle | `00` / `01` |
| 7–8 | Pausa entre vueltas | La pausa real es ≈ **0,53 × el valor** |
| 9 | Sensibilidad | El mando la ignora: siempre `00` |

Comportamiento medido:

| Modo | Sin bucle | Con bucle |
|---|---|---|
| **Pulsar** | Cada pulsación ejecuta la macro entera una vez | **Interruptor:** un toque la pone a repetirse sin parar hasta el siguiente toque. Si se mantiene el botón, se repite mientras se mantiene |
| **Mantener** | Avanza solo mientras se mantiene el botón | Se repite mientras se mantiene y se detiene al soltar |

| Pausa entre vueltas escrita | Pausa real medida |
|---|---|
| 500 | ~263 ms |
| 2000 | ~1050 ms |

### Borrar una macro

`53 FC` deja el botón limpio con una sola trama: eventos y ajustes a cero y, si ejecutaba la macro, sin asignar. Si el botón tenía una tecla asignada, no la toca.

## Aviso `53 FB`

El mando lo envía por su cuenta al configurar un botón trasero con el atajo M + botón: `53 FB 06 <botón> <estado> sum8`.

| Momento | Estado |
|---|---|
| Al entrar en configuración y al pulsar el destino | `00` |
| Al guardar | `01` |

Si la app no está conectada, el aviso espera en el endpoint y llega al conectar. Puede llegar mientras se espera otra respuesta: hay que filtrarlo.

## Turbo

El turbo (M + botón; el anillo parpadea rojo al activar y verde al desactivar) **no se puede leer ni escribir por GCM**. No aparece en ninguna consulta y el comando candidato (`56`) no responde. Solo se configura con los botones del mando.

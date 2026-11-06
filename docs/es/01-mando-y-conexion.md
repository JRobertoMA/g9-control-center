# El mando y su conexión

[English](../01-controller-and-connection.md) · **Español**

## Identificación

| Campo | Valor |
|---|---|
| Producto | Lenovo Legion Gaming Controller G9 2026 (modelo LGC910) |
| Fabricante real | GameSir (Guangzhou Xiaoji Technology) |
| VID:PID (modo Android) | `3537:1136` |
| Nombre USB | `Legion Gaming Controller G9 2026` |
| Nombre como dispositivo de entrada | `GameSir Legion Gaming Controller G9 2026` (+ `… Consumer Control`) |
| Alimentación | Por USB‑C desde la tablet (5 V, 0,5 A), sin batería |
| Firmware probado | 0.10 · protocolo GCM 2.8 |
| Modo de actualización (DFU) | PID `2003`, nombre `…G9 2026_DFU` 🔍 (no se usa) |

## Interfaces USB en modo Android

| Interfaz | Clase | Endpoint | Dirección | Uso |
|---|---|---|---|---|
| 0 | HID | `0x81` | IN | Entradas del gamepad |
| 0 | HID | `0x01` | OUT | Salida del gamepad (vibración del sistema) |
| **1** | HID | **`0x83`** | **IN** | **Respuestas de configuración (GCM)** |
| **1** | HID | **`0x03`** | **OUT** | **Comandos de configuración (GCM)** |

Todos los endpoints son *interrupt* de 64 bytes.

- La configuración va por la **interfaz 1**. Las tramas GCM se escriben **tal cual**, sin report ID ni relleno hasta 64 bytes.
- En Android basta con la API USB Host estándar (`claimInterface` + `bulkTransfer`, que también sirve para endpoints *interrupt*) y el permiso USB normal. No hace falta root.
- Al reclamar la interfaz 1, el gamepad (interfaz 0) sigue funcionando con normalidad.

### Con auriculares enchufados

Al enchufar algo en el jack de 3,5 mm, el mando **se desconecta y se vuelve a enumerar** con tres interfaces USB Audio delante de las dos HID:
- control de audio;
- salida de 48 kHz, 16 bits, estéreo, por `0x04`;
- entrada de micrófono de 48 kHz, 16 bits, mono, por `0x84`.

El canal GCM pasa a ser la interfaz 4. Al desenchufar, vuelve a las dos HID. Ver [audio](05-fallos-y-limitaciones.md#jack-de-auriculares).

## Modos de conexión

El mando tiene tres modos. Al cambiar de modo **se desconecta y vuelve a conectarse** con otra identidad USB:

| Modo | LED | VID:PID | Identidad USB | Configuración GCM |
|---|---|---|---|---|
| **Android** | Azul oscuro | `3537:1136` | GameSir / Legion Gaming Controller G9 2026 | ✅ Sí |
| **Xbox** | Verde | `045E:028E` | Fabricante “2.4G” / Xbox 360 Controller (XInput) | ❌ No |
| **DS5** | Blanco | `054C:0CE6` | Sony Interactive Entertainment / Legion Gaming Controller G9 2026 | ❌ No |

- El color del LED de cada modo se muestra en el **LED del Home**. Si la luz del Home tiene la saturación y el brillo bajos, al cambiar de modo **no se ve el parpadeo de color**: el LED sigue apagado, aunque el modo cambia igual ([detalle](03-ajustes.md#luz-del-home-05-08)).
- La **configuración solo se puede leer y escribir en modo Android**, pero se guarda en el mando y **se aplica en los tres modos**. Por ejemplo, una zona muerta de 50 escrita en Android actúa igual en Xbox y en DS5.
- **G‑Touch** (probado en el G9 2026): `08 02 07 01 02 01` lo activa, y al mantener Vista + Menú 2 s se sale de él. El mando aparece entonces ante Android como una pantalla táctil de 10 dedos (X/Y 0–4095), y repite como toques reales lo que le envía una app: `13 <n.º> [(id<<4 | estado) xL xH+(yL<<4) yH]… sum8` (estado 1 = pulsado, 0 = soltar). En este modo, los botones y los sticks no llegan a Android: llegan por el canal de configuración, en informes `43 …`. G9 Control no usa este modo. El comando `13` no está en la lista de comandos que la app oficial declara para estos mandos; se probaron un toque, dos dedos y un arrastre.

### Cambiar de modo

| Desde | A | Cómo |
|---|---|---|
| Cualquiera | Siguiente | Mantener **Vista + Menú 2 s**. Ciclo: Android → Xbox → DS5 → Android |
| Android | Xbox | Comando `08 02 07 01 08 01` ✅ (ver abajo) |
| DS5 | Android | Paquete HID de salida de 48 bytes por la interfaz 0: `02 00 04 00 … 00 FF FF FF` ✅ |
| Xbox | — | No hay canal de configuración: solo con los botones |

**Cambio de modo por comando** (`08 02 07 01 <modo principal> <submodo> sum8`):

| Petición | Resultado |
|---|---|
| `08 02 07 01 08 01 1B` (modo 8, que corresponde a “DS5” en otros mandos GameSir) | El mando pasa a **Xbox**, no a DS5 ✅ |
| `08 02 07 01 0B 01 1E` (modo 11, “X‑INPUT”) | Rechazado: responde `08 02 07 01 01 01 14` y sigue en Android ✅ |
| `08 02 07 01 01 01 14` (modo 1, submodo 1) | Sin cambio de modo; restablece el submodo ✅ |

Desde Android no hay forma conocida de pasar directamente a DS5: hay que pasar a Xbox y pulsar Vista + Menú.

### Lectura del modo (`08 02`)

`08 02 05 00 0F` → `08 02 07 00 <principal> <submodo> sum8`. En Android se lee principal `01` (HID+GCM).

| Submodo | Nombre | Notas |
|---|---|---|
| `01` | PROTOZOA | Valor de fábrica |
| `02` | GCM | — |
| `03` | GAME_HUB | **Lo escribe la app oficial al conectarse.** En este submodo el mando envía además su estado de entrada por la interfaz 1 (paquetes de 64 bytes que empiezan por `43`). Volver a `01` con `08 02 07 01 01 01` lo detiene |

## Comportamiento en la tablet (Lenovo Legion Y700 Gen5)

- **Al pasar a Xbox**, Android ofrece abrir “Asistente móvil” (*Mobile Assistant* de Lenovo/Motorola), que se registra para **cualquier** dispositivo USB. G9 Control se registra para el G9 en modo Xbox (`045E:028E` + fabricante “2.4G”) y en modo DS5 (`054C:0CE6` + nombre del G9), así que se puede elegir y marcar “Abrir siempre”.
- **“Suministro de alimentación al dispositivo conectado a través de USB”:** es una notificación normal de Android y aparece en cada reconexión. No hay que cambiar sus “Preferencias de USB”: cambiar el papel del puerto puede cortar la comunicación con el mando.

## Atajos del mando (comprobados contra el protocolo)

| Función | Atajo | Ajuste afectado |
|---|---|---|
| Asignar botón trasero | M + botón trasero → botón destino → botón trasero | Remapeo (solo cabe **una** asignación con el atajo) |
| Gatillo rápido | M + LT/RT 2 s (interruptor en recorrido largo) | Gatillo rápido |
| Distribución A↔B, X↔Y | M + A/B/X/Y 2 s | Distribución |
| Efecto de los anillos | M + stick izq. ← → | Efecto |
| Brillo / saturación / color de los anillos | M + stick izq. ↑↓ / M + cruceta ↑↓ / M + girar el stick | Color HSB |
| Vibración | M + Vista (−) / M + Menú (+) | Ambas empuñaduras, pasos de 25 |
| Turbo | M + botón (M + Home 2 s borra todos) | No se puede leer ni escribir por GCM |
| Calibración | Vista + Menú + Home → mover sticks y gatillos → A | Solo con los botones |
| Cambio de modo | Vista + Menú 2 s | Ver arriba |

# Ajustes del mando

[English](../03-settings.md) · **Español**

Todas las escrituras de este documento se verificaron en el mando: escritura → confirmación → nueva lectura con el valor esperado. Los valores **se guardan en el mando**, sobreviven a la reconexión y se aplican en los modos Android, Xbox y DS5.

## Información del mando

### `01 01`

Petición de 2 bytes, sin `len` ni checksum. Respuesta de 20 bytes, por ejemplo:

```
C9 00 00 00 00 00 00 01 00 00 00 0A 00 14 17 0B 15 08 02 00
```

| Bytes | Campo | Ejemplo |
|---|---|---|
| 0 | Cabecera `C9` | — |
| 1–6 | MAC | vacía (el G9 2026 no tiene Bluetooth) |
| 7–8 | Hardware (`b8.b7`) | 0.1 |
| 9–10 | Versión USB (`b10.b9`) | 0.0 |
| 11–12 | Firmware (`b12.b11`) | 0.10 |
| 13–16 | Fecha de fabricación | `14 17 0B 15` → 2023/11/21 |
| 17–18 | Versión GCM (`b18.b17`) | 2.8 |
| 19 | Batería | 0 (sin batería) |

**Fecha:** los bytes 13 y 14 son el siglo y el año en decimal concatenados (`20`·`23` = 2023), no un entero de 16 bits.

### `50`

Petición de 1 byte. Respuesta de 17 bytes: `50`, fecha (4 bytes, mismo formato), **ID de chip** (9 bytes, único de cada mando), ID de modelo (`16` = 22), ID de color (`00`) y `sum8`.

## Zonas muertas (`07`)

Leer: `07 <zona> 06 00 00 sum8`. Escribir: `07 <zona> 06 01 <valor> sum8`. El mando responde con el eco.

| Zona | Ajuste | Rango en G9 Control | Fábrica |
|---|---|---|---|
| `01` / `02` | Stick izq. / der. · zona interna | 0–50 | 10 |
| `03` / `04` | Stick izq. / der. · zona externa | 50–100 | 100 |
| `05` / `06` | LT / RT · zona inicial | 0–50 | 5 |
| `07` / `08` | LT / RT · zona final | 50–100 | 95 |

Los valores son porcentajes del recorrido. No se ha comprobado qué hace el mando con valores fuera de esos rangos. La zona interna se aplica **reescalando**: la salida sube suave desde 0, sin salto. Ejemplo: `07 01 06 01 0F 1E` pone la zona interna del stick izquierdo en 15.

## Gatillo rápido (`21`)

Leer: `21 00 01 01 23` → `21 00 <LT> <RT> sum8`. Escribir: `21 01 <LT> <RT> sum8`, con `01` = activado y `02` = desactivado. Necesita el interruptor físico del gatillo en **recorrido largo**.

## Funciones de la cruceta (`23`)

`23 <función> 06 <acción> <0/1> sum8`:

| Función | Ajuste |
|---|---|
| `01` | Bloqueo de diagonales de la cruceta |
| `04` | Intercambiar stick izquierdo ↔ cruceta |

Ejemplo: `23 01 06 01 01 2C` activa el bloqueo de diagonales.

## Vibración (`25 01`)

Leer: `25 01 0D 00` + 8 × `00` + `33`. Escribir: `25 01 0D 01` + 8 bytes + `sum8` (eco).

| Byte | Campo |
|---|---|
| 4 / 8 | Intensidad de la empuñadura izq. / der. (0–100) |
| 5–7 / 9–11 | Vibración de los gatillos (intensidad, al pulsar, sincronizada). El G9 2026 guarda `00`: no parece tener motores en los gatillos |

La interfaz oficial usa **niveles 0–4** (= 0, 25, 50, 75, 100). El atajo M + Vista/Menú cambia las dos empuñaduras en pasos de 25.

## Anillos de luz (`05 0C`)

Los anillos de los sticks son las “tiras” 1 (izquierdo) y 2 (derecho).

| Operación | Trama | Respuesta |
|---|---|---|
| Leer | `05 0C 06 00 <tira> sum8` | `05 0C 0C 00 <tira> H S B <efecto> <velocidad> <b10> sum8` |
| Escribir | `05 0C 0C 01 <tira> H S B <efecto> <velocidad> <b10> sum8` | Acuse `05 0C 07 01 <tira> 01 sum8` (no es un eco) |
| Valores de fábrica | `05 0C 06 02 00 19` (las dos tiras) | Los valores restablecidos |

- **Color HSB:** H 0–255 (= 0–360°), S 0–100, B 0–100. **B es el brillo real.**
- **Efectos:** `01` fijo · `02` respiración · `04` arcoíris · `FF` apagado. El atajo M + stick ← → los recorre en ese orden.
- **Velocidad:** 0–255 (solo en efectos animados). La interfaz oficial la muestra en %: `byte = (% × 255 + 50) / 100`.
- **Byte 10:** la app oficial lo llama “brillo”, pero **el mando ignora su escritura** y no tiene efecto visible. Solo cambia con el restablecer de fábrica (que lo pone en 128).
- ⚠️ **El mando redondea el color hacia abajo al guardarlo:** al escribir H 179 S 100 B 25 se lee 178 / 100 / 24. Para obtener el valor exacto hay que escribir el objetivo más la diferencia observada (180 → se guarda 179). Al verificar, conviene una tolerancia de ±2.

## Luz del Home (`05 08`)

| Operación | Trama |
|---|---|
| Leer | `05 08 06 00 C2 D5` → `05 08 0A 00 C2 H S B 01 sum8` |
| Escribir | `05 08 0A 01 C2 H S B 01 sum8` (eco) |
| Valores de fábrica | `05 08 06 02 C2 D7` |

`C2` es el código del botón Home. Único efecto: fijo (`01`). El brillo mínimo que guarda el mando es **1**: al escribir `05 08 0A 01 C2 00 00 00 01 DB` (B 0) se lee B 1.

> ⚠️ **El aviso de color del modo usa el LED del Home.** Si bajas la **saturación y el brillo** de la luz del Home, al cambiar de modo **no se ve el parpadeo** de color de Xbox (verde), DS5 (blanco) o Android (azul): el LED del Home sigue apagado. El cambio de modo ocurre igual. Para volver a ver el aviso, sube la saturación y el brillo del Home. (Observado por un usuario con el firmware 0.10.)

## Distribución de botones (`1E`)

Leer: `1E 00 1E` → `1E <valor> sum8`. Escribir: `1E 10 2E` (estándar) o `1E 04 22` (intercambiada: A↔B, X↔Y). El mando responde con el eco. Equivale al atajo M + A/B/X/Y 2 s.

## Valores de fábrica

La función “Restore default” de la app oficial **no usa un comando de reinicio**: reescribe cada ajuste con su valor por defecto. Esos valores son:

| Ajuste | Valor de fábrica |
|---|---|
| Sticks · zona interna / externa | 10 / 100 |
| Gatillos · zona inicial / final | 5 / 95 |
| Gatillo rápido, bloqueo de diagonales, stick izq. ↔ cruceta | Desactivados |
| Vibración de las empuñaduras | 75 (nivel 3) |
| Anillos | H 127 (≈179°, cian), S 100, B 50, fijo, velocidad 128 (con `05 0C 06 02 00`) |
| Luz del Home | H 0, S 0, B 100: blanco (con `05 08 06 02 C2`) |
| Distribución | Estándar (`1E 10`) |
| Botones traseros | Sin asignar y sin macros (`53 FC` y `1F 02` en cada uno) |

G9 Control aplica exactamente esta lista y verifica cada valor (19 escrituras).

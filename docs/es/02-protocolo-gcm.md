# Protocolo GCM

[English](../02-gcm-protocol.md) · **Español**

**GCM** es el protocolo de configuración de los mandos GameSir. En el G9 2026 viaja por la interfaz USB 1: comandos por el endpoint `0x03` y respuestas por el `0x83` ([conexión](01-mando-y-conexion.md)).

## Trama estándar

| Byte | Campo | Descripción |
|---|---|---|
| 0 | `cmd` | Comando (área) |
| 1 | `sub` | Subcomando o índice |
| 2 | `len` | Longitud **total** de la trama, checksum incluido |
| 3 | `action` | `00` leer · `01` escribir · `02` restablecer (según el comando) |
| 4 … n−2 | datos | Parámetros |
| n−1 | `sum8` | Checksum |

**Checksum:** suma de todos los bytes anteriores, truncada a 8 bits.

```
07 01 06 00 00 0E        leer la zona muerta interna del stick izquierdo
│  │  │  │  │  └─ sum8 = 07+01+06+00+00 = 0E
│  │  │  │  └──── dato (vacío al leer)
│  │  │  └─────── action 00 = leer
│  │  └────────── len 6
│  └───────────── sub 01
└──────────────── cmd 07 (zonas muertas)
```

## Variantes

| Comando | Forma |
|---|---|
| `01 01` información | 2 bytes, sin `len` ni checksum. Responde con 20 bytes que empiezan por `C9` |
| `50` identificador | 1 byte, sin checksum |
| `21` gatillo rápido | `21 <sub> <LT> <RT> sum8` (sin `len`) |
| `1E` distribución | `1E <sub> sum8` |
| `1F` remapeo | `1F <operación> <botón> <destino> <extra> sum8` (sin `len`) |
| `53` macros | `53 <sub> <len> <botón> … sum8`. El byte 3 es el botón, no la acción |

## Petición y respuesta

- El mando responde por `0x83`. La respuesta empieza por el mismo `cmd` que la petición, salvo la información, que empieza por `C9`.
- **Las escrituras se confirman con un eco** de la trama (o con un acuse corto: anillos y macros). La forma fiable de verificar una escritura es **volver a leer** el ajuste.
- El mando puede enviar **paquetes no solicitados**: el aviso `53 FB` al configurar un botón con el atajo, y el estado de entrada `43 …` en el submodo GAME_HUB. Conviene aceptar como respuesta solo paquetes con el `cmd` esperado y el checksum correcto.
- Con un tiempo de espera de unos 400 ms y 3 reintentos, las 31 lecturas de la configuración completa tardan menos de un segundo.

## Comandos del G9 2026

El mando solo responde a una lista concreta de comandos. Los que no están en ella, como `08 03` (tipo de entrada) o `56` (asignación de teclas con turbo), **no responden**.

| Cmd | Sub | Acción | Función | Estado | Detalle |
|---|---|---|---|---|---|
| `01` | `01` | — | Información (firmware, GCM, fabricación) | ✅ | [Ajustes](03-ajustes.md#información-del-mando) |
| `50` | — | — | Identificador (fecha, ID de chip, modelo, color) | ✅ | [Ajustes](03-ajustes.md#información-del-mando) |
| `08` | `02` | `00`/`01` | Modo principal y submodo | ✅ | [Conexión](01-mando-y-conexion.md#cambiar-de-modo) |
| `07` | `01`–`08` | `00`/`01` | Zonas muertas de sticks y gatillos | ✅ | [Ajustes](03-ajustes.md#zonas-muertas-07) |
| `21` | `00`/`01` | — | Gatillo rápido | ✅ | [Ajustes](03-ajustes.md#gatillo-rápido-21) |
| `23` | `01`, `04` | `00`/`01` | Bloqueo de diagonales; stick izq. ↔ cruceta | ✅ | [Ajustes](03-ajustes.md#funciones-de-la-cruceta-23) |
| `25` | `01` | `00`/`01` | Vibración | ✅ | [Ajustes](03-ajustes.md#vibración-25-01) |
| `05` | `0C` | `00`/`01`/`02` | Anillos de luz (`02` = valores de fábrica) | ✅ | [Ajustes](03-ajustes.md#anillos-de-luz-05-0c) |
| `05` | `08` | `00`/`01`/`02` | Luz del Home (`02` = valores de fábrica) | ✅ | [Ajustes](03-ajustes.md#luz-del-home-05-08) |
| `1E` | `00`, `04`, `10` | — | Distribución de botones | ✅ | [Ajustes](03-ajustes.md#distribución-de-botones-1e) |
| `1F` | `00`/`01`/`02` | — | Remapeo de botones traseros | ✅ | [Botones](04-botones-y-macros.md#remapeo-1f) |
| `53` | `FE`, `FD`, `FC`, `00`… | — | Macros | ✅ | [Botones](04-botones-y-macros.md#macros-53) |
| `53` | `FB` | — | Aviso espontáneo al configurar un botón con el atajo | ✅ | [Botones](04-botones-y-macros.md#aviso-53-fb) |
| `15` | `01`/`02` | — | “Restaurar configuración”: formato desconocido | ❓ no usado | — |

> ⚠️ Algunos comandos **parecen consultas y no lo son**: `53 FC` borra una macro, `1F 02` deja un botón sin asignar y `53 FD … 02` restablece los ajustes de una macro.

> ⛔ No envíes comandos de actualización de firmware ni nada que ponga el mando en modo DFU. No se documentan aquí.

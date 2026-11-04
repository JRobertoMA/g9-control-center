<p align="center"><img src="assets/icon-512.png" width="128" alt="Icono de G9 Control"></p>

# G9 Control

[English](README.md) · **Español**

App Android ligera y **no oficial** para configurar el **Lenovo Legion Gaming Controller G9 2026** (fabricado por GameSir, USB `3537:1136`) conectado por USB‑C a una tablet Android. Es una alternativa a la app oficial *Ultra Control* que solo incluye la configuración del mando: sin cuentas, sin publicidad y sin analíticas. Solo se conecta a internet cuando tú lo pides: para **enviar un diagnóstico** o para **buscar actualizaciones de la app** ([política de privacidad](PRIVACIDAD.md)). La app está en **inglés y español**, según el idioma del sistema.

Este repositorio contiene:

- la **app compilada** (sección *Releases*);
- la **documentación de los hallazgos**: el protocolo de configuración del mando, sus ajustes y los fallos del firmware encontrados.

El código fuente no se publica por ahora.

> ⚠️ Proyecto no oficial, sin relación con Lenovo ni GameSir. Úsalo bajo tu propia responsabilidad. Lee el **[aviso legal](AVISO-LEGAL.md)** antes de instalarla.

## Descarga

| Versión | Archivo | SHA‑256 |
|---|---|---|
| 0.10.0‑beta.4 (versión de prueba) | `G9Control-0.10.0-beta.4.apk` (en *Releases*) | `f76295eddb5b8a4a04eed426c4da4a492ea9db012aa260336cdf408843a19bad` |
| 0.8.0 | `G9Control-0.8.0.apk` (en *Releases*) | `b004fc7c4bffaa020eb291a90c79106455aa28b3ce7489be17fb71e564b513df` |

Comprueba el archivo antes de instalarlo: `sha256sum G9Control-0.10.0-beta.4.apk`. Novedades de cada versión: [cambios](CHANGELOG.es.md).

**Requisitos:** Android 12 o superior con USB Host (OTG), y un mando Legion conectado por USB‑C (el G9 2026, para configurarlo todo). No necesita root, Shizuku ni cuenta. Probada en una Lenovo Legion Y700 Gen5 (TB323FU) con el firmware 0.10 del mando (protocolo GCM 2.8).

## Mandos soportados

| Mando | USB (modo Android) | Estado |
|---|---|---|
| **Legion Gaming Controller G9 2026** | `3537:1136` | ✅ Verificado: se puede configurar todo |
| Legion Gamepad G9 无极 (Wuji) | `3537:1161` | 🧪 Experimental. Verificado por un probador (desde la 0.10.0‑beta.3): sticks, vibración, luces y botones. El cambio de modo sigue en solo lectura |
| Legion Gaming Controller G9 (2025) | `3537:1134` | 🧪 Experimental. Verificado por probadores (desde la 0.10.0‑beta.3): sticks, vibración, botones y cambio de modo. Las luces siguen en solo lectura |
| Legion Gaming Controller G9 Pro | solo por el nombre | 🧪 Experimental (añade botones ópticos, aún sin soporte) |
| Legion Gaming Controller G7 | `3537:1129` | 🧪 Experimental (sin luces, vibración ni macros; solo L4 y R4) |
| Legion Gaming Controller G3 | `3537:1154` | 🔎 Solo se detecta: la app oficial no tiene comandos de configuración para él |

G9 Control solo envía a cada mando los comandos que la app oficial declara para ese modelo, y nunca toca un mando en modo de actualización de firmware (DFU).

### Ayuda a añadir tu mando

Si tienes uno de los mandos experimentales:
1. Conéctalo en modo Android.
2. Entra en **Mando → Enviar diagnóstico**: verás exactamente lo que se envía, y nunca incluye el ID de chip.
3. Si quieres, deja un contacto de Reddit, Telegram o correo.

Con eso se prueba contigo cada área, paso a paso, antes de desbloquearla. Detalles: [política de privacidad](PRIVACIDAD.md).

## Qué puede configurar

| Área | Funciones |
|---|---|
| Sticks y gatillos | Zonas muertas interna/externa de cada stick, zonas inicial/final de cada gatillo, gatillo rápido, bloqueo de diagonales de la cruceta, intercambio stick izq. ↔ cruceta |
| Vibración y luces | Vibración por empuñadura (niveles 0–4); anillos de luz (efecto, color, saturación, brillo, velocidad); luz del botón Home |
| Botones | Remapeo de los 4 botones traseros (todos a la vez), macros de hasta 11 teclas con modo pulsar/mantener y bucle |
| Mando | Información, paso a modo Xbox, distribución ABXY, perfiles con nombre, restaurar desde respaldo, valores de fábrica |
| Prueba | Pasos de calibración y vista en vivo a pantalla completa de sticks, gatillos y botones |
| Uso de la tablet | Opcional: combinaciones del mando y asa en pantalla para abrir notificaciones, ajustes rápidos, recientes, inicio y atrás en apps a pantalla completa |

Cada cambio se escribe en el mando, se confirma y **se vuelve a leer para verificarlo**. La configuración se guarda en el propio mando y funciona en los modos Android, Xbox y DS5.

**Lo que no hace:** actualizar el firmware, modo G‑Touch, Bluetooth, mandos fuera de la familia Legion (GameSir…).

## Documentación

| Documento | Contenido |
|---|---|
| [Guía de uso](docs/es/06-guia-de-uso.md) | Cómo usar la app sección por sección, usar la tablet con el mando puesto y preguntas frecuentes |
| [Política de privacidad](PRIVACIDAD.md) | Qué envía «Enviar diagnóstico», adónde va y cuánto tiempo se guarda |
| [El mando y su conexión](docs/es/01-mando-y-conexion.md) | Identidad USB, interfaces, modos Android/Xbox/DS5 y cómo cambiar entre ellos |
| [Protocolo GCM](docs/es/02-protocolo-gcm.md) | Transporte, formato de trama, checksum y catálogo de comandos |
| [Ajustes](docs/es/03-ajustes.md) | Información del mando, sticks, gatillos, vibración, luces, distribución y valores de fábrica |
| [Botones y macros](docs/es/04-botones-y-macros.md) | Remapeo, macros y sus límites |
| [Fallos y limitaciones](docs/es/05-fallos-y-limitaciones.md) | Comportamientos anómalos del firmware y cómo evitarlos |

Leyenda de estado en los documentos: ✅ verificado en el mando · 🔍 deducido, sin verificar · ❌ no soportado.

## Licencia

- **Documentación:** [CC BY‑SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.es) — puedes compartirla y adaptarla, incluso con fines comerciales, citando la fuente y manteniendo la misma licencia.
- **App (APK):** uso gratuito y redistribución sin modificar; no se permiten versiones modificadas ni su venta.

Detalles: [LICENCIA](LICENCIA.md).

[![CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-sa/4.0/deed.es)

## Aviso

Lenovo y Legion son marcas de Lenovo. GameSir es marca de Guangzhou Xiaoji Technology. Xbox es marca de Microsoft. DualSense y PlayStation son marcas de Sony Interactive Entertainment. Se citan solo para identificar el hardware compatible. Ver el [aviso legal](AVISO-LEGAL.md) completo.

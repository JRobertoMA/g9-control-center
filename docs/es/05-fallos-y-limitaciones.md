# Fallos del firmware y limitaciones

[English](../05-firmware-issues-and-limitations.md) · **Español**

Comportamientos anómalos encontrados con el firmware 0.10 (GCM 2.8). G9 Control los tiene en cuenta; cualquier otra herramienta que hable GCM con este mando debería hacerlo también.

## Macros de más de 11 eventos

El mando admite 12 eventos repartidos en dos paquetes (`01` con 11 y `FF` con el último), pero:

1. **El acuse del último paquete es defectuoso:** `53 FF 0A 03 00 32`, que son los primeros bytes de la petición, sin checksum válido.
2. **El mando se cuelga ⚠️.** Después de leer una macro de varios paquetes, la siguiente lectura de eventos (`53 FE`), del mismo botón o de otro, no recibe respuesta, y **unos 16 segundos después el mando se reinicia** (se desconecta y vuelve a conectarse). Leer la macro hasta el final no lo evita. Solo se arregla reiniciando el mando (desconectar y conectar). Las demás consultas siguen funcionando mientras tanto.
3. Una escritura acusada justo antes del cuelgue **puede no guardarse**.

En las pruebas no se perdió la configuración en ninguno de los reinicios.

**Lectura de varios paquetes:** pedir el paquete n devuelve el n+1 (`53 FE … 00` → paquete `01`; `53 FE … 01` → paquete `FF`).

**Recomendación:** usar macros de **11 eventos como máximo** (un solo paquete, índice `00`). Si una macro de varios paquetes ya existe (por ejemplo, creada con la app oficial), léela una vez y no vuelvas a enviar `53 FE` hasta reconectar el mando. Después bórrala con `53 FC`.

## Checksum +1 en el remapeo de un botón con macro

La lectura del remapeo de un botón que ejecuta una macro llega como `1F 00 <botón> FF FF <sum8 + 1>`. Por ejemplo, `1F 00 03 FF FF 21` cuando la suma da `20`. Si se exige el checksum correcto, esa respuesta se descarta y la lectura falla. Hay que aceptarla como caso especial.

## El color de los anillos se redondea

Al guardar un color, el mando lo redondea hacia abajo (H 179 → 178, B 25 → 24), y cada reescritura con el valor leído lo oscurece un poco más. Para obtener un valor exacto, escribe el objetivo más la diferencia observada. Verifica con una tolerancia de ±2.

## Byte 10 de los anillos

El mando ignora la escritura de ese byte (la app oficial lo llama “brillo”). Solo el restablecer de fábrica lo cambia (a 128). El brillo real es la B del color.

## El valor de modo “DS5” lleva a Xbox

`08 02 07 01 08 01`, el valor que otros mandos GameSir usan para DS5, pone el G9 2026 en **modo Xbox**. El valor 11 (X‑INPUT) se rechaza. No hay forma conocida de pasar de Android a DS5 directamente. Desde Xbox no hay canal de configuración: la vuelta es con Vista + Menú.

## Submodo GAME_HUB

La app oficial pone el submodo `03` (GAME_HUB) cada vez que se conecta. En ese submodo el mando envía además **su estado de entrada por el canal de configuración** (paquetes de 64 bytes que empiezan por `43`). Hay que ignorarlos o volver al submodo `01` con `08 02 07 01 01 01`.

## El aviso de modo depende de la luz del Home

El parpadeo de color que indica el modo (verde Xbox, blanco DS5, azul Android) sale por el LED del Home. Con la **saturación y el brillo del Home bajos**, ese aviso **no se ve**: el LED se queda apagado al cambiar de modo, aunque el cambio se produce. Sube la saturación y el brillo del Home si quieres ver el aviso ([luz del Home](03-ajustes.md#luz-del-home-05-08)).

## Jack de auriculares

Probado con tres auriculares distintos:

| Modo | Auriculares |
|---|---|
| **Android** | ✅ Funcionan: Android ve el mando como unos auriculares USB (salida y micrófono) |
| Xbox | ❌ Sin sonido: el mando no ofrece audio; suena la tablet |
| DS5 | ❌ Sin sonido: el mando ofrece audio como un DualSense, pero Android solo crea el micrófono; suena la tablet |

**El volumen es bajo** incluso con Android al máximo:
- Home + cruceta ↑/↓ solo envía teclas de volumen normales a Android;
- la app oficial no tiene ningún comando de audio;
- el volumen propio de la tarjeta de sonido USB no es accesible sin root.

G9 Control no puede subirlo.

## La app oficial puede sobrescribir la configuración

Usar “Restore default” en la app oficial devuelve el mando a los [valores de fábrica](03-ajustes.md#valores-de-fábrica) sin más aviso. Si alternas entre apps, guarda antes un respaldo o un perfil.

## Limitaciones del mando

| Función | Estado |
|---|---|
| Dos teclas a la vez en un botón trasero (por remapeo o macro) | ❌ No soportado |
| Turbo | Solo con los botones del mando; no se puede leer ni escribir |
| Calibración | Solo con los botones del mando (Vista + Menú + Home) |
| Giroscopio | No tiene |
| Vibración de los gatillos | No parece tener motores en los gatillos |
| Interruptores de recorrido de los gatillos | Mecánicos; no se pueden leer |
| Tipo de entrada (`08 03`), asignación con turbo (`56`) | No responden |
| Comando `15` (“restaurar configuración”) | Formato desconocido; no se usa |
| Apagar el mando a voluntad | ❌ No hay orden de apagado; el comando de tiempo de reposo (`26`) no responde; cortar la alimentación USB requiere root. Se duerme tras 10 minutos sin uso. Sí puedes apagar sus luces (anillos en «Apagado», Home al mínimo) |
| Actualización de firmware | Fuera del alcance de este proyecto |

## Avisos conocidos

- **G9 (2025), anillos de luz:** un probador vio encendidos solo los LEDs del lado izquierdo cuando la app puso los dos anillos en rojo fijo, aunque el mando indicó que había guardado el rojo en los dos. Los anillos de otro probador funcionaron bien. Aún no se conoce la causa.
- **G9 (2025), sin probar aún:** *Cambiar a modo Android* (desde DS5) y *Valores de fábrica*. Si *Valores de fábrica* indica que los anillos no se verificaron, puede que el restablecimiento sí funcionara: la app compara con los valores de fábrica del G9 2026. Exporta antes un respaldo.

# Guía de uso de G9 Control

[English](../06-user-guide.md) · **Español**

G9 Control configura el **Legion Gaming Controller G9 2026 y G9 (2025)** conectado por USB‑C a una tablet Android (probada en la Lenovo Legion Y700 Gen5). No necesita root ni cuenta, y solo usa internet si pulsas **Enviar diagnóstico**. Toda la configuración se guarda en el propio mando y funciona en sus tres modos: Android, Xbox y DS5.

La app está en **inglés y español** y sigue el idioma del sistema. En Android 13+ puedes elegir el idioma solo para esta app: *Ajustes → Apps → G9 Control → Idioma*.

## Primeros pasos

1. Descarga `G9Control-<versión>.apk` de *Releases*, comprueba su SHA‑256 e instálala (Android 12 o superior; Android pedirá permitir la instalación de apps de origen desconocido).
2. Conecta el mando por USB‑C en **modo Android** (LED azul oscuro).
3. Android pregunta si quieres abrir G9 Control: acepta y, en el aviso de acceso USB, pulsa **Permitir**. Si tocas fuera del aviso cuenta como denegado: pulsa **Conectar** para que vuelva a aparecer.
4. Al conectar, la app **lee toda la configuración** (sin modificar nada) y cada sección del menú lateral muestra los valores actuales.

> Consejo: antes de cambiar nada, pulsa **Exportar respaldo JSON** (sección **Avanzado**). «Restaurar respaldo» usa siempre el respaldo más reciente.

Cada cambio se escribe en el mando, se confirma y **se vuelve a leer para verificarlo**. El resultado aparece en la línea de estado: “hecho y verificado”.

## Secciones del menú lateral

En las secciones de ajustes, el botón flotante **Restaurar respaldo** (abajo a la derecha) vuelve a escribir los ajustes de esa sección con el respaldo más reciente.

### Sticks
- **Zona muerta** de cada stick con un deslizador de dos tiradores: el izquierdo es la **zona interna** (recorrido que se ignora alrededor del centro, 0–99) y el derecho la **zona externa** (a partir de qué punto se considera el tope, 1–100; 100 = sin recorte). Como en la app oficial, la interna siempre queda por debajo de la externa.
- **Cruceta:** bloqueo de diagonales e intercambio stick izquierdo ↔ cruceta.

### Gatillos
- **Zona inicial y final** de cada gatillo, con el mismo deslizador de dos tiradores.
- **Gatillo rápido** de LT y RT (requiere el interruptor trasero del gatillo en recorrido largo).

### Vibración
- Intensidad de las empuñaduras en niveles 0–4, las dos a la vez o cada una por separado. El mando solo guarda esos 5 niveles (0, 25, 50, 75 y 100 %).
- **Los juegos solo pueden hacer vibrar el mando en modo DS5:** en los modos Android y Xbox, Android no ve sus motores. La intensidad que elijas se aplica también en DS5.
- **Probar vibración:** con el mando en modo DS5, la tarjeta *Probar vibración* hace vibrar un momento los dos motores (o cada uno), con la intensidad guardada.
- **Emuladores (PS2 y otros):** si la vibración es demasiado fuerte, el nivel 1 (25 %) es lo más suave que aún vibra. Para bajarla más, usa la escala de vibración del propio emulador.

### Luces
- **Anillos de luz** de los sticks: efecto (fijo, respiración, arcoíris, apagado), color, saturación, brillo y velocidad, juntos o por separado.
- **Luz del botón Home.**

### Botones
- Dibujo del mando con lo que tiene asignado cada botón trasero.
- **Distribución de botones:** estándar o intercambiada (A↔B, X↔Y).
- **Botones traseros** (L4, R4, L5, R5): asigna a cada uno cualquier botón o dirección de stick. Desde la app caben las cuatro asignaciones a la vez; con el atajo del mando (M + botón), solo una.
- **Macros:** hasta 11 teclas seguidas en un botón trasero, cada una con su pausa (desde que se soltó la anterior) y el tiempo que queda pulsada.
  - **Pulsar:** cada pulsación ejecuta la macro entera.
  - **Mantener:** la macro avanza solo mientras mantienes el botón.
  - **Bucle:** repite la macro. Con “Pulsar”, un toque la activa y otro la detiene. La pausa real entre vueltas es aproximadamente la mitad del valor escrito; la app la muestra al lado.
- ⚠️ **Un botón trasero no puede pulsar dos teclas a la vez** (por ejemplo L3 + R3 para el menú de RetroArch). Asigna una tecla a cada botón trasero (L4 → L3, R4 → R3) y púlsalos juntos.

### Mando
- **Información:** firmware, protocolo, fecha de fabricación e identificador del mando.
- **Modo de conexión:** “Pasar a modo Xbox”. En Xbox la app no puede configurar el mando ni devolverlo a Android. Para volver, mantén **Vista + Menú 2 s** dos veces (Xbox → DS5 → Android), o una vez y pulsa **Cambiar a modo Android** en la app. DS5 se alcanza desde Xbox con un Vista + Menú.
- **Perfiles:** guarda la configuración actual con un nombre y aplícala cuando quieras. Se pueden exportar a un archivo e importar (por ejemplo, para compartirlos).
- **Restaurar:** todo desde el último respaldo, o **valores de fábrica**: sticks 10–100, gatillos 5–95, vibración nivel 3, anillos cian, Home blanco, distribución estándar y botones traseros sin asignar.
- **Usar la tablet con el mando:** combinaciones del mando y asa en pantalla para abrir notificaciones, ajustes rápidos, recientes, inicio y atrás sin deslizar desde el borde (ver más abajo).

### Prueba
- **Calibración** con los botones del mando (la app no puede calibrar por USB):
  1. Interruptores de los gatillos en recorrido largo.
  2. Mantén **Vista + Menú + Home** hasta que el LED del Home parpadee.
  3. Gira cada stick a tope 2–3 vueltas.
  4. Aprieta cada gatillo a fondo 3 veces.
  5. Pulsa **A**.
- **Iniciar prueba en vivo:** abre a pantalla completa los sticks (con sus zonas muertas dibujadas), los gatillos y los botones tal como los recibe Android. Mientras está abierta, el mando no actúa sobre la app. Para salir, **mantén Home unos 2 segundos** (o usa el gesto Atrás).

### Avanzado
- **Vigilar cambios:** relee el mando cada pocos segundos y resalta lo que cambió (útil para ver qué cambia un atajo del mando).
- **Exportar respaldo JSON.**
- **Lectura:** todos los valores leídos del mando, con las tramas en crudo. Útil para comprobar o para informar de un problema.
- **Consola y registro:** consola hexadecimal para enviar comandos GCM a mano y registro de todo el tráfico con el mando.

## Usar la tablet con el mando puesto

En apps a pantalla completa, el marco del mando dificulta deslizar desde el borde para ver las barras del sistema. G9 Control incluye un servicio de accesibilidad propio que lo resuelve:

| Mantén Vista y pulsa | Acción |
|---|---|
| Y | Notificaciones |
| X | Ajustes rápidos |
| LB | Recientes |
| A | Inicio |
| B | Atrás |
| RB | Mostrar u ocultar el asa |
| R3 | Rodea para buscar |

- **Rodea para buscar:** Vista + R3 o el asa simulan la pulsación larga en la barra de navegación (en apps a pantalla completa, antes la muestran). Requiere la navegación por gestos.
- **Asa en pantalla:** un medio círculo en el borde (como el del lápiz); al tocarlo abre esas acciones y se puede arrastrar por el borde. Solo aparece con el mando conectado.
- **Activarlo:** sección **Mando** → tarjeta «Usar la tablet con el mando» → **Activar en Accesibilidad**. Si Android dice que el ajuste está restringido (la app no viene de una tienda), abre **Ficha de la app** → ⋮ → «Permitir ajustes restringidos» y vuelve a intentarlo.
- **Privacidad:** el servicio solo mira los botones del G9; no lee la pantalla, no guarda nada y no usa internet; solo simula los gestos de la barra de navegación.

## Prueba guiada (mandos experimentales)

Si tu mando muestra la banda roja de «soporte experimental», pulsa en ella **Prueba guiada**. Con el mando conectado en modo Android y su configuración leída, la prueba:

1. Guarda un respaldo.
2. Recorre cada ajuste, un paso cada vez:
   - escribe un valor de prueba y lo relee;
   - si hace falta, te pide comprobarlo (por ejemplo, ¿se ven rojos los anillos?) o pulsar un botón, que detecta sola;
   - vuelve a poner tu valor.
3. Al final relee todo y lo compara con el respaldo.
4. Desbloquea en tu tablet las áreas que pasaron, para que puedas cambiar esos ajustes normalmente.
5. Te deja **enviar los resultados**.

**Detener y restaurar** lo deja todo como estaba en cualquier momento. El paso del cambio de modo es opcional: para volver del modo Xbox, mantén Vista + Menú 2 s dos veces.

## Actualizar la app

Sección **Mando** → **Actualizaciones de la app**:
- **Buscar actualizaciones** busca una versión más nueva en las *Releases* de este repositorio;
- «Incluir versiones de prueba (beta)» ofrece también las betas;
- «Comprobar al abrir la app» (desactivado por defecto) lo hace solo, como mucho una vez al día.

**Descargar e instalar** verifica el archivo (SHA‑256 y firma) y abre la confirmación de Android. La primera vez, Android pide permitir que G9 Control instale apps. Se conservan tus ajustes, perfiles y respaldos.

La primera versión con el actualizador (0.10.0‑beta.1) hay que instalarla a mano.

## Enviar diagnóstico

Sección **Mando** → **Enviar diagnóstico** (también en el aviso de los mandos con soporte experimental):
- muestra exactamente lo que se enviará y deja añadir un contacto (Reddit, Telegram o correo) para que el desarrollador te responda;
- no se envía nada hasta que pulses **Enviar**, y nunca incluye el ID de chip del mando;
- tras el envío, la app muestra un id;
- **Exportar** guarda el mismo archivo para compartirlo tú.

Detalles: [política de privacidad](../PRIVACIDAD.md).

## Preguntas frecuentes

**Al pasar a Xbox, Android ofrece abrir “Asistente móvil”.** Es una app de Lenovo que se ofrece con cualquier dispositivo USB. Elige G9 Control y marca “Abrir siempre” para que se abra directamente.

**Al cambiar de modo, el LED del Home no parpadea en verde, blanco o azul.** El aviso de color del modo sale por la luz del Home: si le has bajado la saturación y el brillo, se queda apagado. El modo cambia igual; sube la saturación y el brillo del Home (sección **Luces**) si quieres ver el aviso.

**Aparece la notificación “Suministro de alimentación al dispositivo conectado a través de USB”.** Es normal: la tablet alimenta el mando. No cambies las opciones de “Preferencias de USB”.

**He usado la app oficial (Ultra Control) y mi configuración ha cambiado.** “Restore default” de la app oficial devuelve el mando a los valores de fábrica. Restaura tu respaldo o aplica un perfil desde la sección **Mando**.

**Las macros de 12 teclas creadas con la app oficial.** Pueden colgar y reiniciar el mando ([fallo del firmware](05-fallos-y-limitaciones.md#macros-de-más-de-11-eventos)). G9 Control lo detecta y deja de leer macros hasta que reconectes el mando. Bórrala y guárdala de nuevo con 11 teclas como máximo.

**Actualizar el firmware.** No está disponible en G9 Control: usa la app oficial.

## Archivos

- El registro del tráfico también se guarda en `Android/data/dev.g9control/files/registro/`.
- Respaldos: `Android/data/dev.g9control/files/respaldos/`. Perfiles: `…/files/perfiles/`.

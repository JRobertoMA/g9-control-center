# Guía de uso de G9 Control

[English](../06-user-guide.md) · **Español**

G9 Control configura el **Legion Gaming Controller G9 2026** conectado por USB‑C a una tablet Android (probada en la Lenovo Legion Y700 Gen5). No necesita root, cuenta ni conexión a internet. Toda la configuración se guarda en el propio mando y funciona en sus tres modos: Android, Xbox y DS5.

La app está en **inglés y español** y sigue el idioma del sistema. En Android 13+ puedes elegir el idioma solo para esta app: *Ajustes → Apps → G9 Control → Idioma*.

## Primeros pasos

1. Descarga `G9Control-<versión>.apk` de *Releases*, comprueba su SHA‑256 e instálala (Android 12 o superior; Android pedirá permitir la instalación de apps de origen desconocido).
2. Conecta el mando por USB‑C en **modo Android** (LED azul oscuro).
3. Android pregunta si quieres abrir G9 Control: acepta y, en el aviso de acceso USB, pulsa **Permitir**. Si tocas fuera del aviso cuenta como denegado: pulsa **Conectar** para que vuelva a aparecer.
4. Al conectar, la app **lee toda la configuración** (sin modificar nada) y cada pestaña muestra los valores actuales.

> Consejo: antes de cambiar nada, pulsa **Exportar respaldo JSON**. “Restaurar” usa siempre el respaldo más reciente.

Cada cambio se escribe en el mando, se confirma y **se vuelve a leer para verificarlo**. El resultado aparece en la línea de estado: “hecho y verificado”.

## Pestañas

### Sticks y gatillos
- **Zona interna** de cada stick: recorrido que se ignora alrededor del centro. **Zona externa**: punto a partir del cual se considera el tope (100 = sin recorte).
- **Zona inicial y final** de cada gatillo.
- **Gatillo rápido** de LT y RT (requiere el interruptor trasero del gatillo en recorrido largo).
- **Cruceta:** bloqueo de diagonales e intercambio stick izquierdo ↔ cruceta.

### Vibración y luces
- **Vibración** de cada empuñadura en niveles 0–4.
- **Anillos de luz** de los sticks: efecto (fijo, respiración, arcoíris, apagado), color, saturación, brillo y velocidad, juntos o por separado.
- **Luz del botón Home.**

### Botones
- **Botones traseros** (L4, R4, L5, R5): asigna a cada uno cualquier botón o dirección de stick. Desde la app caben las cuatro asignaciones a la vez; con el atajo del mando (M + botón), solo una.
- **Macros:** hasta 11 teclas seguidas en un botón trasero, cada una con su pausa (desde que se soltó la anterior) y el tiempo que queda pulsada.
  - **Pulsar:** cada pulsación ejecuta la macro entera.
  - **Mantener:** la macro avanza solo mientras mantienes el botón.
  - **Bucle:** repite la macro. Con “Pulsar”, un toque la activa y otro la detiene. La pausa real entre vueltas es aproximadamente la mitad del valor escrito; la app la muestra al lado.
- ⚠️ **Un botón trasero no puede pulsar dos teclas a la vez** (por ejemplo L3 + R3 para el menú de RetroArch). Asigna una tecla a cada botón trasero (L4 → L3, R4 → R3) y púlsalos juntos.

### Mando
- **Información:** firmware, protocolo, fecha de fabricación e identificador del mando.
- **Modo de conexión:** “Pasar a modo Xbox”. En Xbox la app no puede configurar el mando ni devolverlo a Android. Para volver, mantén **Vista + Menú 2 s** dos veces (Xbox → DS5 → Android), o una vez y pulsa **Cambiar a modo Android** en la app. DS5 se alcanza desde Xbox con un Vista + Menú.
- **Distribución de botones:** estándar o intercambiada (A↔B, X↔Y).
- **Perfiles:** guarda la configuración actual con un nombre y aplícala cuando quieras. Se pueden exportar a un archivo e importar (por ejemplo, para compartirlos).
- **Restaurar:** todo desde el último respaldo, o **valores de fábrica**: sticks 10–100, gatillos 5–95, vibración nivel 3, anillos cian, Home blanco, distribución estándar y botones traseros sin asignar.

### Prueba
- **Calibración** con los botones del mando (la app no puede calibrar por USB):
  1. Interruptores de los gatillos en recorrido largo.
  2. Mantén **Vista + Menú + Home** hasta que el LED del Home parpadee.
  3. Gira cada stick a tope 2–3 vueltas.
  4. Aprieta cada gatillo a fondo 3 veces.
  5. Pulsa **A**.
- **Prueba en vivo:** sticks (con sus zonas muertas dibujadas), gatillos y botones tal como los recibe Android. En esta pestaña los botones del mando no actúan sobre la app.

### Lectura
Todos los valores leídos del mando, con las tramas en crudo. Útil para comprobar o para informar de un problema.

## Preguntas frecuentes

**Al pasar a Xbox, Android ofrece abrir “Asistente móvil”.** Es una app de Lenovo que se ofrece con cualquier dispositivo USB. Elige G9 Control y marca “Abrir siempre” para que se abra directamente.

**Al cambiar de modo, el LED del Home no parpadea en verde, blanco o azul.** El aviso de color del modo sale por la luz del Home: si le has bajado la saturación y el brillo, se queda apagado. El modo cambia igual; sube la saturación y el brillo del Home (pestaña **Vibración y luces**) si quieres ver el aviso.

**Aparece la notificación “Suministro de alimentación al dispositivo conectado a través de USB”.** Es normal: la tablet alimenta el mando. No cambies las opciones de “Preferencias de USB”.

**He usado la app oficial (Ultra Control) y mi configuración ha cambiado.** “Restore default” de la app oficial devuelve el mando a los valores de fábrica. Restaura tu respaldo o aplica un perfil desde la pestaña **Mando**.

**Las macros de 12 teclas creadas con la app oficial.** Pueden colgar y reiniciar el mando ([fallo del firmware](05-fallos-y-limitaciones.md#macros-de-más-de-11-eventos)). G9 Control lo detecta y deja de leer macros hasta que reconectes el mando. Bórrala y guárdala de nuevo con 11 teclas como máximo.

**Actualizar el firmware.** No está disponible en G9 Control: usa la app oficial.

## Avanzado

- **Mostrar consola y registro:** enseña la consola hexadecimal (para enviar comandos GCM a mano) y el registro de todo el tráfico con el mando. El registro también se guarda en `Android/data/dev.g9control/files/registro/`.
- Respaldos: `Android/data/dev.g9control/files/respaldos/`. Perfiles: `…/files/perfiles/`.

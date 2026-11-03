# Política de privacidad

[English](PRIVACY.md) · **Español**

G9 Control no tiene publicidad, analíticas, rastreadores ni cuentas, y no envía nada por su cuenta. Solo se conecta a internet en dos casos, los dos cuando tú lo pides: **«Enviar diagnóstico»** y **buscar actualizaciones de la app**.

## Enviar diagnóstico

El botón está en la sección **Mando** y en el aviso de los mandos con soporte experimental. Abre un diálogo con:
- la **vista previa exacta de lo que se enviará**;
- un contacto opcional para que el desarrollador te responda: eliges **Reddit, Telegram o correo**;
- el botón **Enviar**.

No sale nada de la tablet hasta que pulses **Enviar**.

**Qué se envía** (un archivo JSON):

| Dato | Ejemplo |
|---|---|
| Mando | Modelo, modo de conexión, ID de fabricante y de producto USB y su nombre, fabricante, y sus interfaces USB (clase y endpoints) |
| Dispositivos de audio | El tipo y el nombre de los dispositivos de audio que ve Android (por ejemplo «auriculares USB»), para revisar el jack del mando |
| Tablet y app | Versión de Android, marca y modelo de la tablet, versión de G9 Control |
| Configuración del mando | Todo lo que leyó la app (zonas muertas, luces, remapeos, macros…), **sin el ID de chip del mando** |
| Registro de la sesión | El tráfico de la app con el mando, **sin las líneas del ID de chip** |
| Contacto | Solo si escribiste uno: el tipo (Reddit, Telegram o correo) y tu usuario o dirección |

**Qué no se envía:** tus cuentas, archivos, ubicación, otras apps, la pantalla ni el identificador único del mando (ID de chip). El servidor rechaza cualquier envío que lo contenga.

**Adónde va:**
1. El diagnóstico va a un servidor del desarrollador de G9 Control (`diag.yuutomugetsu.com`, alojado en IONOS, en la UE).
2. El servidor se lo reenvía al desarrollador mediante un bot de Telegram.
3. Tu dirección IP solo se usa para limitar los envíos (5 por hora) y no se guarda: el contador usa un resumen irreversible que cambia cada día.

**Para qué:** dar soporte a más mandos y corregir fallos. No se comparte con nadie más.

**Cuánto tiempo se guarda:** **90 días**; después se borra automáticamente.

**Borrarlo antes:** tras enviar, la app muestra un **id** (por ejemplo `G9_2026-144407-454a82`). Pide al desarrollador que lo borre indicando ese id, en los *Issues* de este repositorio.

## Actualizaciones de la app

En la sección **Mando**, la tarjeta **Actualizaciones de la app** busca en GitHub Releases de este repositorio una versión más nueva de G9 Control:
- solo cuando pulsas **Buscar actualizaciones**;
- o al abrir la app, si activas «Comprobar al abrir la app» (**desactivado por defecto**), como mucho una vez al día.

La app solo *lee* la información pública de las releases en GitHub (`api.github.com` y sus servidores de descarga). No envía nada sobre ti, tu mando ni tu tablet. Como cualquier web, GitHub ve tu dirección IP y la trata según [su propia declaración de privacidad](https://docs.github.com/es/site-policy/privacy-policies/github-general-privacy-statement).

Antes de instalar, la app comprueba que el archivo coincide con el publicado (SHA‑256) y que está firmado con la misma clave, y Android siempre te pide confirmar.

## Todo lo demás se queda en tu dispositivo

Los respaldos, perfiles, registros y diagnósticos exportados se guardan en el almacenamiento de la app en tu tablet. Los respaldos incluyen el ID de chip del mando: no los compartas. Los perfiles y los diagnósticos no lo incluyen.

**Servicio de accesibilidad (opcional):** «Usar la tablet con el mando». Solo filtra las teclas del G9 y simula los gestos de la barra de navegación (por ejemplo, para «Rodea para buscar»). No lee la pantalla ni otras apps, no registra nada y no envía nada.

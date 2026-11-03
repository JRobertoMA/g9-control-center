# Aviso legal y descargo de responsabilidad

[English](DISCLAIMER.md) · **Español**

Lee este aviso antes de descargar, instalar o usar **G9 Control** o la información de este repositorio. Al hacerlo, aceptas sus condiciones.

## 1. Proyecto no oficial

G9 Control es un proyecto **independiente, personal y sin ánimo de lucro**. **No está afiliado, patrocinado, respaldado ni aprobado** por Lenovo, GameSir / Guangzhou Xiaoji Technology, Microsoft, Sony Interactive Entertainment ni ninguna otra empresa.

Lenovo, Legion, GameSir, Xbox, DualSense, PlayStation y el resto de marcas citadas pertenecen a sus respectivos titulares. Se mencionan solo para identificar el hardware con el que la app es compatible.

## 2. Origen de la información

La información técnica de este repositorio se obtuvo:

- observando el comportamiento del mando conectado a un dispositivo propio (tráfico USB, respuestas a comandos y eventos de entrada);
- analizando la app oficial, con el **único fin de lograr la interoperabilidad** entre el mando y una app alternativa.

Este repositorio **no contiene ni distribuye** código, binarios, recursos, firmware, manuales ni capturas de pantalla de terceros. Las tablas y ejemplos de tramas son propios. No se ha eludido ninguna medida de protección.

## 3. Sin garantía

La app y la documentación se ofrecen **“tal cual”**, **sin garantía de ningún tipo**, expresa o implícita. Eso incluye, entre otras, las garantías de comerciabilidad, idoneidad para un fin concreto, exactitud y ausencia de errores.

La información puede ser **incompleta o incorrecta**. Se probó con un único mando y un único dispositivo: firmware 0.10 del mando, protocolo GCM 2.8, tablet Lenovo Legion Y700 Gen5. Otros firmwares, revisiones de hardware o versiones de Android pueden comportarse de forma distinta.

## 4. Uso bajo tu propia responsabilidad

Usar G9 Control o enviar comandos al mando **es responsabilidad exclusiva de quien lo hace**. Ten en cuenta que:

- La app **modifica la configuración guardada en el mando**. Guarda un respaldo antes de cambiar nada.
- El firmware del mando tiene fallos conocidos que pueden **colgarlo y reiniciarlo**, o hacer que **se pierda** una escritura (ver [Fallos y limitaciones](docs/es/05-fallos-y-limitaciones.md)).
- La consola hexadecimal permite enviar **cualquier comando**. Un comando incorrecto puede dejar el mando en un estado inesperado. Úsala solo si sabes lo que haces.
- G9 Control **no actualiza el firmware** ni entra en el modo de actualización. No intentes hacerlo con herramientas no oficiales.
- Usar software no oficial **podría afectar a la garantía** del fabricante. Consulta sus condiciones.

## 5. Limitación de responsabilidad

En la máxima medida que permita la ley aplicable, **los autores no serán responsables de ningún daño**, directo, indirecto, incidental, especial o consecuente, que se derive del uso o de la imposibilidad de usar G9 Control o la información de este repositorio. Esto incluye, entre otros:

- daños al mando, a la tablet o a otros dispositivos;
- pérdida de configuración o de datos;
- mal funcionamiento en juegos o aplicaciones;
- pérdida de garantía.

## 6. Privacidad

G9 Control no contiene publicidad ni analíticas y no envía nada por su cuenta. Solo usa internet cuando pulsas **Enviar diagnóstico**, tras una vista previa de exactamente lo que se envía (datos del mando y de la tablet, configuración y registro, **sin el ID de chip**); se guarda 90 días. Detalles: [política de privacidad](PRIVACIDAD.md). Los respaldos, perfiles y registros se guardan en el almacenamiento de la app en tu dispositivo. Los respaldos incluyen el identificador único del mando (ID de chip): no los compartas. Los perfiles no lo incluyen, así que se pueden compartir.

El **servicio de accesibilidad** opcional («Usar la tablet con el mando») solo filtra las teclas de un mando G9 para ejecutar acciones del sistema (notificaciones, ajustes rápidos, recientes, inicio, atrás), simula el gesto de la barra de navegación que abre «Rodea para buscar» y dibuja el asa en pantalla. No lee la pantalla ni el contenido de otras apps, no registra teclas y no guarda nada. Está desactivado hasta que lo actives en los ajustes de Accesibilidad de Android.

## 7. Legislación

Las leyes sobre ingeniería inversa e interoperabilidad varían según el país. Este aviso **no es asesoramiento legal**. Si tienes dudas sobre su aplicación en tu jurisdicción, consulta a un profesional.

## 8. Retirada de contenido

Si eres titular de algún derecho y consideras que este repositorio lo vulnera, abre un *issue* o contacta con el mantenedor. El contenido se revisará y, si procede, se retirará.

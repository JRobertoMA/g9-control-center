# Cambios

[English](CHANGELOG.md) · **Español**

## 0.10.0 — estable

Primera versión estable desde la 0.8.0. Incluye todo lo de las versiones de prueba 0.9 y 0.10 que aparecen abajo.

- **G9 (2025) totalmente soportado:** las luces y la distribución de botones se desbloquean tras las pruebas guiadas de los probadores. Ya no muestra el aviso de experimental.
- Los mensajes que nombraban el G9 2026 a cualquier mando ahora son genéricos.
- Los demás mandos Legion (G9 无极, G9 Pro, G7) siguen con soporte experimental, y el G3 solo se detecta.

## 0.10.0‑beta.4 — 2026‑11‑03 (versión de prueba)

- **Corregido: los ajustes no hacían nada en los mandos aún marcados como experimentales** (G9 (2025), G9 无极), salvo que antes se exportara un respaldo. El único aviso era la línea de estado. Ahora la app guarda el respaldo sola antes del primer cambio de cada sesión.
- **Corregido: el color de los anillos se oscurecía un poco con cada escritura** (introducido en la 0.10.0‑beta.3). Si tus anillos se ven algo más tenues o con otro tono, vuelve a poner su color.

## 0.10.0‑beta.3 — 2026‑11‑03 (versión de prueba)

Gracias a los primeros probadores (G9 无极 y dos G9 (2025)):

- **Desbloqueado tras sus pruebas guiadas:**
  - **G9 无极:** sticks, vibración, luces (con el efecto multicolor) y botones;
  - **G9 (2025):** sticks, vibración, botones y cambio de modo.
  - El resto de áreas sigue en solo lectura hasta que se verifique.
- **Correcciones de la prueba guiada:**
  - las áreas superadas se desbloquean para el mando correcto, aunque al final se quedara en modo Xbox. La prueba espera a que vuelva en modo Android para la comprobación final;
  - los anillos en multicolor o arcoíris se restauran con su color: con esos efectos el mando ignora el color enviado, así que la app lo escribe antes en fijo (puede verse un instante). Los anillos en negro tampoco «fallan» ya;
  - el paso del modo ya no se da por bueno solo cuando llegan pulsaciones de más;
  - cada mando recibe solo los pasos que admite (por ejemplo, el G7 no tiene luces, vibración ni macros).
- **Vibración:**
  - nueva tarjeta *Probar vibración*;
  - los números 0–4 quedan bajo sus marcas;
  - probado: el mando solo guarda 5 niveles (los valores intermedios se redondean hacia abajo), y los juegos solo pueden hacerlo vibrar en modo DS5.
- **Zonas muertas** con los rangos de la app oficial: interna o inicial 0–99 y externa o final 1–100, con la interna siempre por debajo de la externa (antes 0–50 y 50–100, así que un valor puesto con la app oficial no se podía restaurar).
- El servicio de accesibilidad reconoce el mando en modo DS5, donde Android añade «Sony Interactive Entertainment» a su nombre.

## 0.10.0‑beta.2 — 2026‑11‑02 (versión de prueba)

- **Prueba guiada para mandos experimentales** (G9 无极 y otros modelos Legion):
  - prueba cada ajuste paso a paso: escribe un valor de prueba, lo relee, te pide comprobarlo (o detecta el botón solo) y vuelve a poner tu valor;
  - antes guarda un respaldo, y al final lo compara todo con él;
  - las áreas que pasan se desbloquean en tu tablet, y los resultados se envían con un toque.
- El actualizador acepta etiquetas de versión con guiones tipográficos.

## 0.10.0‑beta.1 — 2026‑11‑02 (versión de prueba)

Incluye todo lo de la 0.9.0‑beta.2, que no llegó a publicarse firmada.

- **Actualizador integrado:** busca en GitHub Releases de este repositorio (con el botón, o al abrir la app si lo activas; desactivado por defecto), descarga la APK, verifica su SHA‑256 y su firma, y la instala con la confirmación de Android. Las betas son opcionales.
- **Familia Legion en solo lectura:** se detectan el G9 (2025), el G9 Pro, el G7 y el G3. Cada uno solo recibe los comandos que la app oficial declara para él, y las secciones que no tiene dicen «No disponible en este mando» (por ejemplo, el G7 no tiene luces, vibración ni macros, y solo L4/R4). El G3 solo se detecta.
- **Seguridad:** nunca se abre un mando en modo de actualización de firmware (DFU), y la consola hex no puede enviar comandos fuera de la lista del modelo conectado.
- **Jack de auriculares** documentado: solo funciona en modo Android, y su volumen lo limita el mando.
- **Diagnóstico más completo:** descriptores de las interfaces USB y dispositivos de audio de Android (para soportar mandos nuevos y revisar el jack de auriculares).

## 0.9.0‑beta.2 — no publicada (incluida en la 0.10.0‑beta.1)

- **Nuevo aspecto:** Material 3 Expressive con los colores de tu fondo de pantalla (Material You), siempre oscuro. Menú lateral como el de la app oficial: Botones, Sticks, Gatillos, Vibración, Luces, Mando, Prueba y Avanzado.
- **Zonas muertas con un deslizador de dos tiradores** (interna–externa, inicial–final), como en la app oficial.
- Dibujo del mando con lo que tiene asignado cada botón trasero; efectos de luz con botones conectados.
- **Restaurar respaldo** en un botón flotante en cada sección. *Vigilar cambios* y *Exportar respaldo JSON* pasan a **Avanzado**, con la lectura en crudo y la consola hex.
- **Rodea para buscar** con Vista + R3 o desde el asa (gesto simulado de la barra de navegación; solo con navegación por gestos).
- **Enviar diagnóstico:** desde la app, con vista previa exacta de lo que se envía, contacto opcional (Reddit, Telegram o correo) y sin el ID de chip. Es el único uso de internet, y solo al pulsar Enviar ([política de privacidad](PRIVACIDAD.md)).
- **Home como destino de un botón trasero:** el botón envía al instante la tecla Guide/Home del mando, así que un launcher puede vincularla (p. ej. el menú de acceso rápido de DroidDeck).
- **Prueba en vivo a pantalla completa:** el mando ya no mueve el foco de la app mientras pruebas. Mantén **Home** unos 2 segundos para salir (o usa el gesto Atrás).
- **Usar la tablet con el mando puesto:** servicio de accesibilidad opcional con combinaciones del mando (mantener Vista + Y/X/LB/A/B/RB: notificaciones, ajustes rápidos, recientes, inicio, atrás, asa) y un asa en pantalla como la del lápiz. Solo actúa sobre las teclas del G9 y no lee la pantalla.

## 0.8.0 — 2026‑11‑01

- **App bilingüe:** inglés y español según el idioma del sistema. En Android 13+ se puede elegir el idioma solo para la app (*Ajustes → Apps → G9 Control → Idioma*).
- **Icono propio** (adaptativo, con versión monocroma para los iconos temáticos).
- Las etiquetas se traducen al mostrarlas, así que un cambio de idioma se aplica sin reiniciar la app.
- **Privacidad:** los perfiles ya no incluyen el ID de chip del mando, así que se pueden compartir sin él. Los respaldos sí lo conservan.
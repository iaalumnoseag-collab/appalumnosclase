# Buzón de Cupones

App de clase en píxel art. Es un único archivo, `index.html`, sin instalación.

## Qué hace

- **Alumnos:** entran con su nombre. Ven un calendario con asistencia y tareas del día, su habitación animada con su avatar, un **cuadro en la pared con su propia imagen**, inventario de cupones, buzón con el estado de sus peticiones, tablón de la clase, fondo de ventana y música.
- **Avatar:** se personaliza entero: peinado (9), color de pelo (10), color de camiseta (10), dibujo de camiseta (8), piel, gafas y pantalón. También hay 12 looks rápidos y un botón ALEATORIO.
- **Taza:** al tocar la taza del escritorio, el avatar va a por ella, bebe (con sonido y corazones) y la vuelve a dejar.
- **Música:** 3 melodías chiptune o **un MP3 propio** que suena en bucle. El botón **REMIX** cambia entre 9 remezclas en directo (Nightcore, Slowed + reverb, Lo-fi, 8 bits, Eco espacial, Radio vieja, Turbo, Bajo el agua…). Con Firebase configurado, el MP3 (máximo 8 MB) se guarda en la base de datos y suena en cualquier dispositivo donde entre el alumno; cada dispositivo lo descarga una vez y lo guarda en caché.

## Qué se guarda de cada alumno

Todo queda asociado a su nombre en la base de datos de Firebase, así que al entrar desde cualquier dispositivo lo encuentra igual: avatar personalizado, fondo, música elegida, remezcla, imagen del cuadro, su MP3, cupones, asistencia y peticiones.

**Importante:** GitHub solo aloja la app, no puede guardar datos. Sin la URL de Firebase pegada en `index.html`, cada dispositivo guarda lo suyo por separado.
- **Estilo:** píxel art nocturno (cielo azul, violeta y rosa con tramado, montañas y pinos nevados). El fondo de toda la app está animado: el cielo cambia de color cada pocos segundos y en bucle (rosa, verde, azul, rojo, amarillo, aurora, violeta, atardecer, hielo), los colores ondulan, el puntero ilumina el cielo y los árboles, las estrellas se apartan a su paso y al hacer clic o tocar salen ondas y chispas. Si el dispositivo tiene activado «reducir movimiento», el fondo queda quieto.
- **Profesor (tú):** entras con un código. Pasas lista; al pulsar PRESENTE el alumno recibe 1 cupón (máximo 2 guardados) y suena «El alumno … ha recibido un cupón». También gestionas peticiones (estados, entrega con enlace), alumnos y tareas del día.
- **Cuadro:** el alumno toca el cuadro (o el botón MI CUADRO) y elige una imagen del ordenador o de la galería del móvil. La app la recorta a 4:3 y la reduce (máximo 480×360, JPEG) sin filtros, así se ve nítida y ocupa poco. Las imágenes se guardan aparte de los demás datos (`bc/photos`) y solo se descargan cuando hacen falta. En la pestaña CLASE ves la miniatura de cada alumno y puedes pulsar QUITAR CUADRO.
- **Cupón:** 1 cupón = 1 petición. Estados: Enviado, Recibido, En proceso, Rehaciéndose, Entregado.

## Probarla ahora

Abre `index.html` en el navegador. **Mientras no pegues la URL de Firebase (ver abajo), los datos se guardan solo en el navegador de cada dispositivo**: lo que marque el profesor en su ordenador no lo verán los alumnos en los suyos.

1. Pulsa «Soy el profesor» y crea tu código.
2. Pestaña CLASE: pega los nombres de tus alumnos (uno por línea).
3. Pestaña ASISTENCIA: pulsa PRESENTE y escucha la voz.
4. Sal, entra como alumno escribiendo su nombre y gasta el cupón.

## Base de datos conectada

Esta versión ya está conectada a Firebase (proyecto `buzon-cupones` de la cuenta iaalumnoseag@gmail.com, Realtime Database en Bélgica):
`https://buzon-cupones-default-rtdb.europe-west1.firebasedatabase.app`

Solo hace falta subir `index.html` a GitHub. Lo de abajo explica cómo se configuró, por si algún día hay que repetirlo.

## Publicarla para toda la clase

Para que alumnos y profesor compartan datos desde dispositivos distintos hacen falta dos cosas gratuitas. GitHub Pages **solo aloja el archivo**; los datos se guardan en Firebase.

### 1. Base de datos (Firebase Realtime Database)

1. Entra en https://console.firebase.google.com y crea un proyecto.
2. Menú «Compilación» → «Realtime Database» → «Crear base de datos».
3. En la pestaña «Reglas» pega esto y publica:
   ```json
   { "rules": { "bc": { ".read": true, ".write": true } } }
   ```
4. Copia la URL de la base de datos (termina en `firebasedatabase.app`).
5. Abre `index.html` con un editor de texto y pega la URL en esta línea, cerca del principio del script:
   ```js
   const CONFIG = { FIREBASE_URL: 'https://tu-proyecto-default-rtdb.europe-west1.firebasedatabase.app' };
   ```

### 2. Alojamiento (GitHub Pages)

1. Crea un repositorio nuevo en GitHub (por ejemplo `buzon-cupones`).
2. «Add file» → «Upload files» y sube `index.html`.
3. «Settings» → «Pages» → origen «Deploy from a branch», rama `main`, carpeta `/ (root)`.
4. En un par de minutos tendrás un enlace tipo `https://usuario.github.io/buzon-cupones/`. Pégalo fijado en un canal de Discord.

**Importante:** la primera vez, entra tú antes que nadie como profesor para crear tu código.

## Límites de seguridad (leer)

Es una app de aula, no un sistema con seguridad real:

- Con las reglas abiertas de arriba, cualquiera que tenga la URL de la base de datos puede leerla y escribir en ella. El código de profesor se guarda cifrado (hash), pero un alumno con conocimientos técnicos podría saltárselo.
- Usa solo nombres de pila (o nombre y inicial). No guardes datos personales sensibles.
- Las imágenes de los cuadros se guardan en la misma base de datos: quien tenga su URL podría verlas. Recomienda a los alumnos no subir fotos con datos personales y revisa la pestaña CLASE de vez en cuando.
- Si más adelante lo necesitas más seguro, el siguiente paso es añadir Firebase Authentication y reglas por usuario.

## Cosas a tener en cuenta

- La voz usa el sintetizador del navegador del profesor; suena por el altavoz de tu dispositivo. Los navegadores solo la permiten tras pulsar un botón, y así funciona aquí.
- La música son melodías originales generadas con el navegador; empieza tras el primer toque en la pantalla.
- Los datos se actualizan cada 4 segundos.

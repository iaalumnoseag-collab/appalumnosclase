# Buzón de Cupones

App de clase en píxel art. Es un único archivo, `index.html`, sin instalación.

## Qué hace

- **Alumnos:** entran con su nombre. Ven un calendario con asistencia y tareas del día, su habitación animada con avatar (12 a elegir), inventario de cupones, buzón con el estado de sus peticiones, tablón de la clase, fondo de ventana y música (generada en el navegador).
- **Profesor (tú):** entras con un código. Pasas lista; al pulsar PRESENTE el alumno recibe 1 cupón (máximo 2 guardados) y suena «El alumno … ha recibido un cupón». También gestionas peticiones (estados, entrega con enlace), alumnos y tareas del día.
- **Cupón:** 1 cupón = 1 petición. Estados: Enviado, Recibido, En proceso, Rehaciéndose, Entregado.

## Probarla ahora (modo prueba)

Abre `index.html` en el navegador. Sin configurar nada, los datos se guardan solo en ese navegador.

1. Pulsa «Soy el profesor» y crea tu código.
2. Pestaña CLASE: pega los nombres de tus alumnos (uno por línea).
3. Pestaña ASISTENCIA: pulsa PRESENTE y escucha la voz.
4. Sal, entra como alumno escribiendo su nombre y gasta el cupón.

## Publicarla para toda la clase

Para que alumnos y profesor compartan datos desde dispositivos distintos hacen falta dos cosas gratuitas.

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
- Si más adelante lo necesitas más seguro, el siguiente paso es añadir Firebase Authentication y reglas por usuario.

## Cosas a tener en cuenta

- La voz usa el sintetizador del navegador del profesor; suena por el altavoz de tu dispositivo. Los navegadores solo la permiten tras pulsar un botón, y así funciona aquí.
- La música son melodías originales generadas con el navegador; empieza tras el primer toque en la pantalla.
- Los datos se actualizan cada 4 segundos.

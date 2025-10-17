# Evidencias de la unidad 7
---


## ACTIVIDAD 01: Conexión entre el celular y el computador

1. **¿Qué URL de Dev Tunnels obtuviste?**  
**R//** Obtuve esta URL: `https://juanjo-devtunnel.use2.devtunnels.ms/](https://pt3fpsgv-3000.use2.devtunnels.ms/`. A esta URL le agregue `/mobile/` al final, como me indicaban en los pasos del ejercicio.
 
2. **¿Por qué crees que necesitamos usar esta URL en lugar de http://localhost:3000 o la IP local de tu computador para que el celular se conecte?**  
**R//** Porque `localhost` solo funciona dentro del mismo equipo donde está corriendo el servidor. La URL de Dev Tunnels crea un enlace público que permite que el celular, acceda al servidor local a través de internet.  

3. **Describe brevemente qué hace npm install y npm start.**  
**R//** `npm install` descarga e instala todas las dependencias necesarias del proyecto, como **express** y **socket.io**, para que funcione correctamente.  
Y `npm start` ejecuta el servidor definido en el archivo `server.js`, iniciando la aplicación en el puerto 3000.  

4. **¿Qué mensajes observaste en la terminal del servidor al conectar el cliente de escritorio y el cliente móvil? ¿Eran diferentes los mensajes o identificadores?**  
**R//** Sí, aparecieron mensajes como:  
- *“New client connected”* cada vez que abría una de las aplicaciones.  
- *“Received message => …”* cuando se enviaban datos del celular al escritorio.  
- *“Client disconnected”* cuando cerraba alguna pestaña.  
Cada cliente tenía un identificador diferente, lo que ayudaba al servidor a distinguir entre el móvil y el escritorio.  

5. **Describe el comportamiento observado: ¿Funcionó la interacción? ¿Hubo algún retraso (latencia)?**  
**R//** Sí, la interacción funcionó correctamente. Al mover el dedo en la pantalla del celular, el círculo rojo en el navegador del computador seguía el movimiento en tiempo real. Solo se notaba un leve retraso muy pequeño, pero en general la conexión fue fluida y estable.

---

## ACTIVIDAD 02: Conectividad y comunicación entre dispositivos  

1. **¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente?**  
**R//** Es necesario porque permite que un servidor local (que normalmente solo se puede acceder desde el mismo computador) sea visible en Internet públicamente.  
Funciona como un “puente” que conecta una URL pública con el puerto local donde corre el servidor. Así, cuando el celular accede a la URL del túnel, la solicitud viaja por Internet hasta el servicio de Dev Tunnels, que luego la reenvía a `localhost:3000`. De esa forma, ambos dispositivos se comunican aunque estén en redes diferentes.  

2. **Describe la función de touchMoved() y por qué se usa la variable threshold en el cliente móvil.**  
**R//** La función `touchMoved()` detecta cuando el usuario mantiene un dedo sobre la pantalla y lo mueve. Al llamarlo cada vez, obtiene las coordenadas actuales del toque (`mouseX` y `mouseY`) y las envía al servidor para actualizar la posición del círculo en el escritorio.  
La variable `threshold` se usa para evitar que se envíen demasiados mensajes por movimientos mínimos o temblores del dedo. Solo cuando el cambio de posición supera ese umbral, se envía un nuevo mensaje, optimizando el rendimiento y reduciendo la latencia.  

3. **Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?**

**R//**

A) **Dev Tunnels:**  
  - *Ventajas:* Me permite acceder al servidor desde cualquier red (Wi-Fi o datos móviles) sin tanto enredo. Es seguro y fácil de usar.  
  - *Desventajas:* Depende de una conexión estable a Internet y de los servicios de Microsoft; si el túnel se cierra, el acceso se pierde.  

B) **IP local:**  
  - *Ventajas:* No necesita Internet, solo una red local; puede ser más rápido en entornos cerrados.  
  - *Desventajas:* Solo funciona si ambos dispositivos están en la misma red y puede fallar por firewalls o configuraciones del router.  

4. **📸 Capturas de pantalla**

**R//** Computador.

<img width="1459" height="910" alt="image" src="https://github.com/user-attachments/assets/d4c56122-4c7d-4ae5-a24b-5dffb074a74f" />

**R//** Celular.

<img width="738" height="1600" alt="image" src="https://github.com/user-attachments/assets/341bd57e-21ba-4701-bcde-26aed2a99546" />

**R//** Terminal.

<img width="1271" height="515" alt="image" src="https://github.com/user-attachments/assets/4bcfe9a4-a4e5-44c5-bb54-e95647158d2c" />

**Mira la demostración aquí:**  
[Video demostrativo](https://youtube.com/shorts/SgP_dZ62z74?feature=share)

---

## ACTIVIDAD 03: Análisis del servidor (server.js)

1. **¿Cuál es la función principal de `express.static('public')` en este servidor?**  
**R//** Sirve para que el servidor muestre automáticamente todos los archivos que están en la carpeta **public**, como los HTML, CSS o scripts.  
A diferencia del `app.get('/ruta', …)` que usábamos antes, aquí no hay que crear rutas manualmente, todo se sirve de una forma más simple y directa.  


2. **Explica detalladamente el flujo de un mensaje táctil:**  
**R//** Cuando muevo el dedo en la pantalla del celular, la función `touchMoved()` envía un mensaje con las coordenadas del movimiento.  
El servidor recibe ese mensaje con `socket.on('message')`, lo muestra en la consola y luego lo reenvía a los demás clientes con `socket.broadcast.emit`.  
El escritorio recibe ese mensaje y actualiza la posición del círculo en tiempo real.  
Se usa `socket.broadcast.emit` porque así el mensaje llega a todos menos al que lo envió, evitando que el celular reciba su propio mensaje.  


3. **Si conectaras dos computadores de escritorio y un móvil a este servidor, y movieras el dedo en el móvil, ¿Quién recibiría el mensaje retransmitido por el servidor? ¿Por qué?**  
**R//** Los dos computadores de escritorio recibirían el mensaje, porque el servidor lo envía a todos los demás clientes conectados, excepto al que lo envió (el celular).  


4. **¿Qué información útil te proporcionan los mensajes `console.log` en el servidor durante la ejecución?**  
**R//** Sirven para ver lo que está pasando en el servidor: cuándo se conecta o se desconecta un cliente, y qué mensajes se están enviando.  
De esa forma uno puede comprobar si todo está funcionando bien o si hay algún problema en la comunicación.

---

## ACTIVIDAD 04: Diagrama.


---

## ACTIVIDAD 05: Apply

1. **Código Server.js:**
´´´
// server.js

const express = require('express');
const http = require('http');
const socketIO = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIO(server);
const port = 3000;

app.use(express.static('public'));

io.on('connection', (socket) => {
    console.log('New client connected');
    
    // Retransmite el mensaje
    socket.on('message', (message) => {
        socket.broadcast.emit('message', message);
    });

    socket.on('disconnect', () => {
        console.log('Client disconnected');
    });
});

server.listen(port, () => {
    console.log(`Server is listening on http://localhost:${port}`);
    console.log(`Desktop client: http://localhost:${port}/desktop/index.html`);
    console.log(`Mobile client: http://localhost:${port}/mobile/index.html`);
});
´´´

2. **Código del desktop/sketch.js:**

´´´
// public/desktop/sketch.js

let socket;
let currentX = 0; 
let currentY = 0;
let currentHue = 0; 
let audioStarted = false; 

const mobileWidth = 300; 
const mobileHeight = 400; 
let speakersBg; 

// Variables de Audio
let song;
let amp; 

// --- Precarga: Carga imagen y audio ---
function preload() {
    speakersBg = loadImage('../assets/speakers_background.png', 
        () => console.log('Imagen de fondo cargada.'),
        (err) => console.error('ERROR (Imagen): No se pudo cargar la imagen. Revisa la ruta y el nombre del archivo.', err)
    );
    
    // Carga la canción
    song = loadSound('../assets/pump_up_the_jam.mp3', 
        () => console.log('Canción "Pump Up The Jam" cargada exitosamente.'),
        (err) => console.error('ERROR (Audio): No se pudo cargar el audio. Revisa que p5.sound esté en index.html y la ruta sea correcta.', err)
    );
}

function setup() {
    createCanvas(800, 600); 
    colorMode(HSB, 360, 100, 100, 255);
    background(0); 
    
    amp = new p5.Amplitude();
    if (song && song.isLoaded()) {
        amp.setInput(song);
    }

    socket = io(); 
    
    socket.on('message', (data) => {
        if (data && data.type === 'touch') {
            currentX = data.x;
            // EL TONO (currentHue) SOLO SE ACTUALIZA CON EL MOVIL
            currentHue = map(currentX, 0, mobileWidth, 0, 360);
            currentY = map(data.y, 0, mobileHeight, 0, height);
        }
    });    
}

function draw() {
    let volume = 0;
    if (audioStarted) {
        volume = amp.getLevel();
    }
    
    // 1. DIBUJAR LA IMAGEN DE FONDO
    if (speakersBg) {
        image(speakersBg, 0, 0, width, height); 
    } else {
        background(0); 
    }
    
    // 2. APLICAR LA CAPA DE COLOR (Overlay)
    if (audioStarted) {
        // Mapeamos el volumen (0.0 a ~0.4) al brillo (desde un nivel bajo 60 hasta 100).
        // El brillo (Brillo) ahora PALPITA al ritmo de la música.
        let dynamicBrightness = map(volume, 0, 0.4, 60, 100, true);
        
        // Usamos HSB
        colorMode(HSB, 360, 100, 100, 255);
        
        // Tono (Hue) = Móvil (currentHue)
        // Brillo (Brightness) = Música (dynamicBrightness)
        fill(currentHue, 70, dynamicBrightness, 80); 
        noStroke();
        rect(0, 0, width, height); 
    } else {
        // Si el audio no ha iniciado, muestra la instrucción
        colorMode(RGB, 255);
        fill(255, 255, 0); 
        textSize(30);
        textAlign(CENTER, CENTER);
        text('CLICK PARA INICIAR MÚSICA Y CONTROL', width / 2, height / 2);
    }
    
    // 3. Mostrar Información
    colorMode(RGB, 255);
    fill(255);
    textSize(14);
    textAlign(LEFT, TOP);
    text(`Tono HSB (Móvil): ${currentHue.toFixed(0)}`, 10, 10);
    text(`Brillo (Música): ${volume.toFixed(2)}`, 10, 30);
    text(`Música: ${song && song.isLoaded() ? (audioStarted ? 'Reproduciendo' : 'Esperando Click') : 'Cargando...'}`, 10, 50);
}

// --- Iniciar Audio al Click del Usuario ---
function mouseClicked() {
    if (song && song.isLoaded() && !audioStarted) {
        song.loop(); 
        audioStarted = true;
    }
    return false;
}
´´´

3. **Código del desktop/INDEX.HTML:**

´´´
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lienzo Controlado por Móvil (Desktop)</title>
    
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.0/lib/p5.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.0/lib/addons/p5.sound.min.js"></script>
    
    <script src="https://cdn.socket.io/4.7.5/socket.io.min.js"></script>
    
    <script src="sketch.js"></script>
    
    <style>
        body {
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #000;
            overflow: hidden;
        }
    </style>
</head>
<body>
</body>
</html>
´´´

4. **Código del mobile/sketch.js:**

´´´
// public/mobile/sketch.js

let socket;
const mobileWidth = 300;
const mobileHeight = 400;

function setup() {
    createCanvas(mobileWidth, mobileHeight); 
    socket = io();
}

function draw() {
    background(50);
    
    // Muestra el estado de conexión
    fill(socket && socket.connected ? 'green' : 'red');
    ellipse(width - 20, 20, 10, 10);
    
    fill(255, 255, 0); 
    textAlign(CENTER, CENTER);
    textSize(18);
    text('ARRASTRA PARA CAMBIAR COLOR', width / 2, height / 2 - 20);
    textSize(12);
    text('El escritorio recibirá las coordenadas X.', width / 2, height / 2 + 10);
}

// NUEVA LÓGICA: Envía la posición del arrastre
function touchMoved() {
    if (socket && socket.connected) {
        let touchData = {
            type: 'touch',
            x: mouseX, 
            y: mouseY  
        };
        socket.emit('message', touchData);
    }
    return false; // Bloquea el scroll
}

function touchStarted() {
    return true; 
}
´´´

5. **Código del mobile/INDEX.HTML**

´´´
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.0/lib/p5.min.js"></script>
    <script src="https://cdn.socket.io/4.7.5/socket.io.min.js"></script>
    <script src="sketch.js"></script>
    <title>Mobile p5.js Application</title>
</head>
<body></body>
</html>
´´´





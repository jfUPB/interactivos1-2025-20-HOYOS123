
# Evidencias de la unidad 8

---

# Actividad 1 – Diseño de Sistema Interactivo con Micro:bit, Móvil y Visuales

## 1. Referentes visuales

Para las visuales me inspiré en **instalaciones interactivas minimalistas** donde el color, la luz y el movimiento responden a la interacción física o digital.

**Referentes:**
- **Rafael Lozano-Hemmer – Pulse Room:** traduce pulsos o señales en animaciones de luz.
- **Visuales de conciertos electrónicos (Amon Tobin, Jon Hopkins):** sincronización entre sonido, color y movimiento.

**Paleta conceptual:** color dinámico (variaciones HSB), formas simples (círculo central), y cambios de brillo y tamaño que representan energía o intensidad.

---

## 2. Concepto de las visuales

El concepto principal es la **interconexión del entorno digital**.  
Cada componente del sistema representa un tipo distinto de interacción:

- El **micro:bit** simboliza el **control físico directo** mediante botones.
- El **móvil** representa la **intención y emoción** del usuario, permitiendo ajustar parámetros visuales.
- La **pantalla de escritorio** es la **manifestación visual** de todas esas señales, un espacio donde se expresan los cambios en color, brillo y forma.

**Idea conceptual:** una “energía visual compartida” donde cada dispositivo influye en la forma final de la visualización.

---

## 3. Control del sistema: móvil y micro:bit

| Dispositivo | Acción | Efecto visual |
|--------------|--------|----------------|
| **micro:bit (botón A)** | Envía `"A"` al servidor | Cambia el color base de las visuales |
| **micro:bit (botón B)** | Envía `"B"` al servidor | Restaura el color base (Rojo) |
| **Móvil - deslizador tamaño** | Envía valor de 50–300 | Cambia el tamaño del círculo |
| **Móvil - deslizador brillo** | Envía valor de 0–255 | Cambia la luminosidad del fondo |
| **Móvil - botón “Pulsar”** | Envía `{ action: "pulse" }` | Círculo crece brevemente y cambia de color aleatorio |

---

## 4. Bocetos de interfaces

**Boceto 1 – Interfaz móvil:**  

**Boceto 2 – Visual de escritorio:**  

**Boceto 3 – Micro:bit**  

---

## 5. Diagrama de comunicación del sistema

<img width="1024" height="768" alt="Gráfico diagrama de flujo sencillo versátil formas naranja y azul" src="https://github.com/user-attachments/assets/b2122816-a11c-4562-85fa-743836dd18e1" />

- El **micro:bit** se comunica por **SerialPort (USB)** con el servidor Node.js.  
- El **servidor Node.js** usa **Socket.IO** para conectar en tiempo real el **móvil** y el **desktop**.  
- El **móvil** envía datos (sliders y pulsaciones).  
- El **desktop** recibe los datos y actualiza la visualización en **p5.js**.

---

# Actividad 2 – Proceso y Códigos del Sistema

## Documentación del proceso de construcción

1. **Configuración del servidor**
   - Se creó un servidor Node.js con **Express** para servir los archivos estáticos.
   - Se integró **Socket.IO** para la comunicación en tiempo real entre móvil y escritorio.
   - Se utilizó **SerialPort** para recibir datos del micro:bit.

2. **Cliente móvil**
   - Interfaz HTML con dos sliders (`size`, `brightness`) y un botón “Pulsar”.
   - Envía datos al servidor mediante `socket.emit("mobile", {...})`.

3. **Cliente de escritorio**
   - Implementado con **p5.js**, genera visuales dinámicas basadas en color, brillo y tamaño.
   - Escucha mensajes `socket.on("mobile")` y `socket.on("microbit")` para modificar las visuales en tiempo real.

4. **Conexión del micro:bit**
   - Código en Python envía `"A"` o `"B"` por USB según el botón presionado.
   - Node.js interpreta esos mensajes y los reenvía a los clientes mediante Socket.IO.

---

## Códigos del proyecto

### **server.js**

```js
const express = require("express");
const http = require("http");
const { Server } = require("socket.io");
const { SerialPort } = require("serialport");
const { ReadlineParser } = require("@serialport/parser-readline");

const app = express();
const server = http.createServer(app);
const io = new Server(server);
const port = 3000;

// Servir archivos estáticos
app.use(express.static("public"));

// ------------------ MICROBIT ------------------
const serial = new SerialPort({
  path: "COM4", // Cambiar según el puerto
  baudRate: 115200,
});

const parser = serial.pipe(new ReadlineParser({ delimiter: "\r\n" }));

parser.on("data", (data) => {
  console.log("Microbit dice:", data);
  if (data === "A") io.emit("microbit", { button: "A" });
  if (data === "B") io.emit("microbit", { button: "B" });
});

// ------------------ SOCKET.IO ------------------
io.on("connection", (socket) => {
  console.log("Cliente conectado:", socket.id);

  socket.on("mobile", (data) => {
    console.log("Móvil:", data);
    io.emit("mobile", data);
  });

  socket.on("disconnect", () => {
    console.log("Cliente desconectado:", socket.id);
  });
});

// ------------------ RUTAS ------------------
app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

app.get("/mobile", (req, res) => {
  res.sendFile(__dirname + "/public/mobile.html");
});

// ------------------ INICIAR SERVIDOR ------------------
server.listen(port, () => {
  console.log(`✅ Servidor corriendo en http://localhost:${port}`);
});
```

### **mobile.html**

```
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Control Móvil</title>
  <script src="/socket.io/socket.io.js"></script>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      background: #111;
      color: #fff;
      padding: 30px;
    }
    input[type=range] {
      width: 80%;
      margin: 10px 0 25px 0;
    }
    #pulseBtn {
      width: 150px;
      height: 150px;
      border-radius: 50%;
      background: #ff3366;
      border: none;
      color: white;
      font-size: 1.2em;
      box-shadow: 0 0 20px rgba(255, 51, 102, 0.7);
      transition: transform 0.2s, box-shadow 0.2s;
    }
    #pulseBtn:active {
      transform: scale(0.9);
      box-shadow: 0 0 10px rgba(255, 255, 255, 0.9);
    }
  </style>
</head>
<body>
  <h2>Control Visual</h2>

  <label>Tamaño del círculo: <span id="sizeVal">150</span></label><br>
  <input type="range" id="size" min="50" max="300" value="150"><br>
  
  <label>Brillo: <span id="brightVal">150</span></label><br>
  <input type="range" id="bright" min="0" max="255" value="150">

  <br><br>
  <button id="pulseBtn">Pulsar</button>

  <script>
    const socket = io();

    const sizeSlider = document.getElementById("size");
    const brightSlider = document.getElementById("bright");
    const sizeVal = document.getElementById("sizeVal");
    const brightVal = document.getElementById("brightVal");
    const pulseBtn = document.getElementById("pulseBtn");

    function enviarSliders() {
      socket.emit("mobile", {
        size: parseInt(sizeSlider.value),
        brightness: parseInt(brightSlider.value)
      });
    }

    sizeSlider.addEventListener("input", () => {
      sizeVal.textContent = sizeSlider.value;
      enviarSliders();
    });

    brightSlider.addEventListener("input", () => {
      brightVal.textContent = brightSlider.value;
      enviarSliders();
    });

    pulseBtn.addEventListener("click", () => {
      socket.emit("mobile", { action: "pulse" });
    });
  </script>
</body>
</html>
```

### **index.html (Desktop)**
```
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Visual Interactivo</title>
  <script src="/socket.io/socket.io.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.0/p5.min.js"></script>
</head>
<body>
  <script>
    const socket = io();

    let colorBase = 0;
    let circleSize = 150;
    let brightness = 150;
    let pulseEffect = 0;

    function setup() {
      createCanvas(windowWidth, windowHeight);
      colorMode(HSB);
      noStroke();
      rectMode(CENTER);
    }

    function draw() {
      background(colorBase, 100, brightness);

      let currentSize = circleSize + pulseEffect;
      fill(255);
      ellipse(width / 2, height / 2, currentSize);

      if (pulseEffect > 0) pulseEffect -= 5;
    }

    socket.on("microbit", (data) => {
      if (data.button === "A") colorBase = random(255);
      if (data.button === "B") colorBase = 0;
    });

    socket.on("mobile", (data) => {
      if (data.size) circleSize = data.size;
      if (data.brightness) brightness = data.brightness;
      if (data.action === "pulse") {
        pulseEffect = 100;
        colorBase = random(255);
      }
    });

    function windowResized() {
      resizeCanvas(windowWidth, windowHeight);
    }
  </script>
</body>
</html>
```

### **microbit.py**
```
from microbit import *

while True:
    if button_a.is_pressed():
        print("A")
        sleep(200)
    if button_b.is_pressed():
        print("B")
        sleep(200)
```

# Video de evidencia:

![Video de muestra](https://youtube.com/shorts/xAQY0D_2X-8?feature=share)




# Unidad 1

## 🛠 Fase: Apply

### Actividad 5

#### Explica cómo funciona el sistema físico interactivo que acabamos de crear.

Este sistema conecta una micro:bit con una computadora para poder controlar un círculo en pantalla usando los botones físicos del micro:bit.

Cuando se presionan los botones A o B en la micro:bit, se envía un comando por el puerto serial (por USB) hacia un programa hecho en p5.js. Ese programa recibe los comandos y mueve un círculo en la pantalla hacia la izquierda o la derecha según el botón presionado.

### Actividad 6

#### Enlace al editor de p5.js:

[Enlace](https://editor.p5js.org/HOYOS123/sketches/DjQUwwuP5)

#### Código del programa:

``` js
let port;
let connectBtn;
let connectionInitialized = false;
let x = 200; // Posición inicial del círculo

function setup() {
  createCanvas(400, 400);
  background(220);

  port = createSerial();

  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(80, 300);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220); // Limpiar pantalla cada frame

  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }

  if (port.availableBytes() > 0) {
    let dataRx = port.read(1); // Lee 1 byte
    if (dataRx) {
      let charRx = String.fromCharCode(dataRx[0]); // Convierte a carácter

      if (charRx === "L") {
        x -= 10; // Mover a la izquierda
      } else if (charRx === "R") {
        x += 10; // Mover a la derecha
      }
    }
  }

  x = constrain(x, 0, width); // Mantener dentro del canvas
  ellipse(x, height / 2, 50, 50); // Dibujar el círculo
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}

```

#### Código del Micro:Bit

``` py
from microbit import *

uart.init(baudrate=115200)

while True:
    if button_a.is_pressed():
        uart.write('L')  # Left
        sleep(150)
    elif button_b.is_pressed():
        uart.write('R')  # Right
        sleep(150)
```

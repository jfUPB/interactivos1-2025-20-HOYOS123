# Unidad 3

## 🛠 Fase: Apply

# Actividad 6

#### Crear la bomba en P5.js

### Bomba 3.0 en p5.js 

## Controles

- `a` → Botón A (sube el tiempo)  
- `b` → Botón B (baja el tiempo)  
- `s` → Sacudir (arma la bomba)  
- `r` → Reset (desarma después de explotar)  

## Código

```js
let bombTask;

class BombTask {
  constructor() {
    this.PASSWORD = ['A', 'B', 'A'];
    this.key = new Array(this.PASSWORD.length).fill('');
    this.keyindex = 0;
    this.count = 20;
    this.startTime = millis();
    this.state = 'CONFIG';
  }

  update() {
    if (this.state === 'CONFIG') {
      this.showCounter();

    } else if (this.state === 'ARMED') {
      if (millis() - this.startTime > 1000) {
        this.startTime = millis();
        this.count--;
        if (this.count <= 0) {
          this.state = 'EXPLODED';
        }
      }
      this.showCounter();

    } else if (this.state === 'EXPLODED') {
      // Mostramos explosión como una X roja
      textSize(64);
      fill(255, 0, 0);
      text("X", width/2, height/2);
    }
  }

  pressKey(k) {
    if (this.state === 'CONFIG') {
      if (k === 'a') {
        this.count = min(this.count + 1, 60);
      }
      if (k === 'b') {
        this.count = max(10, this.count - 1);
      }
      if (k === 's') { // shake
        this.startTime = millis();
        this.state = 'ARMED';
      }
    } 
    else if (this.state === 'ARMED') {
      if (k === 'a') {
        this.key[this.keyindex] = 'A';
        this.keyindex++;
      }
      if (k === 'b') {
        this.key[this.keyindex] = 'B';
        this.keyindex++;
      }

      if (this.keyindex === this.key.length) {
        let passIsOK = true;
        for (let i = 0; i < this.key.length; i++) {
          if (this.key[i] !== this.PASSWORD[i]) {
            passIsOK = false;
            break;
          }
        }
        if (passIsOK) {
          this.count = 20;
          this.keyindex = 0;
          this.state = 'CONFIG';
        } else {
          this.keyindex = 0;
        }
      }
    } 
    else if (this.state === 'EXPLODED') {
      if (k === 'r') { // reset
        this.count = 20;
        this.startTime = millis();
        this.state = 'CONFIG';
      }
    }
  }

  showCounter() {
    textSize(48);
    fill(0, 200, 0);
    text(this.count, width/2, height/2);
  }
}

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  bombTask = new BombTask();
}

function draw() {
  background(0);
  bombTask.update();
}

function keyPressed() {
  bombTask.pressKey(key.toLowerCase());
}
```

# Actividad 7

#### Bomba en p5.js + controles del micro:bit

## Código p5.js

```js
let serial;
let bombTask;

class BombTask {
  constructor() {
    this.state = "CONFIG";
    this.time = 10;
    this.sequence = [];
    this.correctSequence = ["a", "b", "b", "a"];
  }

  update() {
    textSize(24);
    fill(255);

    if (this.state === "CONFIG") {
      text("CONFIG MODE", width/2, height/2 - 40);
      text("Tiempo: " + this.time, width/2, height/2);
    }

    else if (this.state === "ARMED") {
      text("ARMED", width/2, height/2 - 40);
      text("Tiempo: " + this.time, width/2, height/2);

      if (frameCount % 60 === 0 && this.time > 0) {
        this.time--;
      }

      if (this.time === 0) {
        this.state = "EXPLODED";
      }
    }

    else if (this.state === "EXPLODED") {
      text("X", width/2, height/2);
    }
  }

  pressKey(k) {
    if (this.state === "CONFIG") {
      if (k === "a" && this.time < 60) {
        this.time++;
      } else if (k === "b" && this.time > 10) {
        this.time--;
      } else if (k === "s") {
        this.state = "ARMED";
        this.sequence = [];
      }
    }

    else if (this.state === "ARMED") {
      if (k === "a" || k === "b") {
        this.sequence.push(k);
        if (this.sequence.length > this.correctSequence.length) {
          this.sequence.shift();
        }
        if (JSON.stringify(this.sequence) === JSON.stringify(this.correctSequence)) {
          this.state = "CONFIG";
          this.time = 10;
          this.sequence = [];
        }
      }
    }

    else if (this.state === "EXPLODED") {
      if (k === "r") {
        this.state = "CONFIG";
        this.time = 10;
        this.sequence = [];
      }
    }
  }
}

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  bombTask = new BombTask();

  serial = new p5.SerialPort();
  serial.list();
  serial.open('/dev/ttyUSB0'); 
  serial.on('data', serialEvent);
}

function draw() {
  background(0);
  bombTask.update();
}

function keyPressed() {
  bombTask.pressKey(key.toLowerCase());
}

function serialEvent() {
  let data = serial.readLine().trim();
  if (data.length > 0) {
    bombTask.pressKey(data);
  }
}
```

## Enlace al editor con el código

[Enlace al código en p5.js](https://editor.p5js.org/HOYOS123/sketches/cpYENY_7O)

## Código del micro:bit
```
from microbit import *
import random

while True:
    if button_a.was_pressed():
        uart.write("a\n")   # Aumentar tiempo
    if button_b.was_pressed():
        uart.write("b\n")   # Disminuir tiempo
    
    if accelerometer.was_gesture("shake"):
        uart.write("s\n")   # Activar bomba
    
    if pin_logo.is_touched():  # Reiniciar si explotó
        uart.write("r\n")
    
    sleep(100)
```

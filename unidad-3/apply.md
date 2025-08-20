# Unidad 3

## 🛠 Fase: Apply

### Actividad 6

#### Crear la bomba en P5.js

# Bomba 3.0 en p5.js 

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

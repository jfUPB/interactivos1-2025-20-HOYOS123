# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modificar](https://editor.p5js.org/generative-design/sketches/P_2_1_2_01)

Código a modificar:

``` js
// P_2_1_2_01
//
// Generative Gestaltung – Creative Coding im Web
// ISBN: 978-3-87439-902-9, First Edition, Hermann Schmidt, Mainz, 2018
// Benedikt Groß, Hartmut Bohnacker, Julia Laub, Claudius Lazzeroni
// with contributions by Joey Lee and Niels Poldervaart
// Copyright 2018
//
// http://www.generative-gestaltung.de
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

/**
 * changing size and position of circles in a grid
 *
 * MOUSE
 * position x          : circle position
 * position y          : circle size
 * left click          : random position
 *
 * KEYS
 * s                   : save png
 */
'use strict';

var tileCount = 20;
var actRandomSeed = 0;

var circleAlpha = 130;
var circleColor;

function setup() {
  createCanvas(600, 600);
  noFill();
  circleColor = color(0, 0, 0, circleAlpha);
}

function draw() {
  translate(width / tileCount / 2, height / tileCount / 2);

  background(255);

  randomSeed(actRandomSeed);

  stroke(circleColor);
  strokeWeight(mouseY / 60);

  for (var gridY = 0; gridY < tileCount; gridY++) {
    for (var gridX = 0; gridX < tileCount; gridX++) {

      var posX = width / tileCount * gridX;
      var posY = height / tileCount * gridY;

      var shiftX = random(-mouseX, mouseX) / 20;
      var shiftY = random(-mouseX, mouseX) / 20;

      ellipse(posX + shiftX, posY + shiftY, mouseY / 15, mouseY / 15);
    }
  }
}

function mousePressed() {
  actRandomSeed = random(100000);
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
}

```

[Enlace a la aplicación modificada](https://editor.p5js.org/HOYOS123/sketches/13l9qqTY3)

Código modificado:

``` js
let tileCount = 20;
let actRandomSeed = 0;

let circleAlpha = 130;
let circleColor;

let xValue = 0;
let yValue = 0;
let aState = 0;
let bState = 0;

let port;
let connectBtn;
let connectionInitialized = false;

function setup() {
  createCanvas(600, 600);
  noFill();
  circleColor = color(0, 0, 0, circleAlpha);

  // botón de conexión
  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(20, 20);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(255);
  translate(width / tileCount / 2, height / tileCount / 2);

  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }

  // leer datos del micro:bit
  let currentString = port.readUntil("\n");
  if (currentString) {
    let splitData = split(trim(currentString), ",");
    if (splitData.length >= 4) {
      xValue = int(splitData[0]);
      yValue = int(splitData[1]);
      aState = int(splitData[2]);
      bState = int(splitData[3]);
    }
  }

  randomSeed(actRandomSeed);
  stroke(circleColor);
  strokeWeight(abs(yValue) / 60);

  for (let gridY = 0; gridY < tileCount; gridY++) {
    for (let gridX = 0; gridX < tileCount; gridX++) {
      let posX = width / tileCount * gridX;
      let posY = height / tileCount * gridY;

      let shiftX = random(-xValue, xValue) / 200;
      let shiftY = random(-xValue, xValue) / 200;

      ellipse(posX + shiftX, posY + shiftY, abs(yValue) / 150, abs(yValue) / 150);
    }
  }

  // Botón A → nueva semilla aleatoria
  if (aState == 1) {
    actRandomSeed = random(100000);
  }

  // Botón B → guardar imagen
  if (bState == 1) {
    saveCanvas("microbit_pattern", "png");
  }

  // actualizar texto del botón
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }
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

## Video

[Video demostratativo](URL)



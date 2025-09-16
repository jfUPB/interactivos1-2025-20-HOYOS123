# Evidencias de la unidad 5

## Actividad 01: Repasa el caso de estudio

---

### 1. ¿Cómo se están comunicando el micro:bit y el sketch de p5.js? ¿Qué datos envía el micro:bit?
R// El micro:bit se comunica con el sketch de p5.js a través de un serial (UART) configurado a 115200 baudios.
Cada 100 ms (10 Hz) envía una línea con cuatro valores separados por comas y terminados en salto de línea:

- xValue: valor del acelerómetro en el eje X.
- yValue: valor del acelerómetro en el eje Y.
- aState: estado del botón A (True/False).
- bState: estado del botón B (True/False).

### 2. ¿Cómo es la estructura del protocolo ASCII usado?
R// El protocolo es tipo ASCII con delimitadores:
<valorX>,<valorY>,<estadoA>,<estadoB>\n

- Los datos son enviados como texto legible.
- Las comas separan los campos.
- El salto de línea \\n indica el final del mensaje.

---

### 3. Parte del código en p5.js donde se leen los datos del micro:bit y se transforman en coordenadas
R//
```js
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
  if (data) {
    data = data.trim();
    let values = data.split(",");
    if (values.length == 4) {
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    }
  }
}
```

### 4. ¿Cómo se generan los eventos A pressed y B released en p5.js?
R// En la función updateButtonStates(newAState, newBState) donde el evento "A pressed" ocurre cuando el estado de A pasa de false a true, y el evento "B released" ocurre cuando el estado de B pasa de true a false. Estos datos permiten controlar la generación de color, tamaño y posición de los gráficos en pantalla.


### 5. Capturas de pantalla de los dibujos hechos.
R// 


## Actividad 02: Caso de estudio micro:bit

---

### 1. Captura el resultado del experimento anterior. (Datos binarios en SerialTerminal) ¿Por qué se ve este resultado?
R// Cuando el micro:bit envía la información en formato binario, estos datos no se pueden leer directamente como texto, ya que están codificados en bytes compactos usando `struct.pack`.  
Si abrimos el SerialTerminal en modo "texto", lo que aparece son símbolos y caracteres raros. Esto pasa porque el terminal intenta mostrar cada byte como si fuera una letra del código ASCII.  

Para interpretar esos datos de manera correcta, no basta con un visor de texto. Lo que necesitamos es un programa que entienda que esos bytes representan valores numéricos. Un ejemplo de esto será en la siguiente actividad con **p5.js**, donde podremos visualizar los datos como enteros binarios y darles un sentido.

### 2. Captura el resultado del experimento anterior. Lo que ves ¿Cómo está relacionado con esta línea de código?
R// El monitor serial muestra símbolos raros porque los datos van en binario.  
`struct.pack` empaqueta los valores en bytes: eficiente para enviar, pero ilegible para leer a simple vista.

### 3. ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII?
R//

### 4. Captura el resultado del experimento. ¿Cuántos bytes se están enviando por mensaje? ¿Cómo se relaciona esto con el formato '>2h2B'? ¿Qué significa cada uno de los bytes que se envían?
R//

### 5. ¿Cómo se verían esos números en el formato '>2h2B'? (Números Positivos y Negativos para valores de "xValue" y "yValue")
R//

### 6. Captura el resultado del experimento. ¿Qué diferencias ves entre los datos en ASCII y en binario? ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII? ¿Qué ventajas y desventajas ves en usar un formato ASCII en lugar de binario?
R//


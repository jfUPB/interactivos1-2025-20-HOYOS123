# Evidencias de la unidad 5
---

# Fase Apply:

## Actividad 01: Repasa el caso de estudio

### 1. ¿Cómo se están comunicando el micro:bit y el sketch de p5.js? ¿Qué datos envía el micro:bit?
**R//** El micro:bit se comunica con el sketch de p5.js a través de un serial (UART) configurado a 115200 baudios.
Cada 100 ms (10 Hz) envía una línea con cuatro valores separados por comas y terminados en salto de línea:

- xValue: valor del acelerómetro en el eje X.
- yValue: valor del acelerómetro en el eje Y.
- aState: estado del botón A (True/False).
- bState: estado del botón B (True/False).

### 2. ¿Cómo es la estructura del protocolo ASCII usado?
**R//** El protocolo es tipo ASCII con delimitadores:
<valorX>,<valorY>,<estadoA>,<estadoB>\n

- Los datos son enviados como texto legible.
- Las comas separan los campos.
- El salto de línea \\n indica el final del mensaje.

### 3. Parte del código en p5.js donde se leen los datos del micro:bit y se transforman en coordenadas
**R//**
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
**R//** En la función updateButtonStates(newAState, newBState) donde el evento "A pressed" ocurre cuando el estado de A pasa de false a true, y el evento "B released" ocurre cuando el estado de B pasa de true a false. Estos datos permiten controlar la generación de color, tamaño y posición de los gráficos en pantalla.

### 5. Capturas de pantalla de los dibujos hechos.

---

## Actividad 02: Caso de estudio micro:bit

### 1. Captura el resultado del experimento anterior. (Datos binarios en SerialTerminal) ¿Por qué se ve este resultado?

<img width="997" height="726" alt="Captura de pantalla 2025-09-17 145954" src="https://github.com/user-attachments/assets/618fd3e1-ab89-4e9a-9581-38436b86baa6" />

**R//** Cuando el micro:bit envía la información en formato binario, estos datos no se pueden leer directamente como texto, ya que están codificados en bytes compactos usando `struct.pack`.  
Si abrimos el SerialTerminal en modo "texto", lo que aparece son símbolos y caracteres raros. Esto pasa porque el terminal intenta mostrar cada byte como si fuera una letra del código ASCII.  

Para interpretar esos datos de manera correcta, no basta con un visor de texto. Lo que necesitamos es un programa que entienda que esos bytes representan valores numéricos. Un ejemplo de esto será en la siguiente actividad con **p5.js**, donde podremos visualizar los datos como enteros binarios y darles un sentido.

### 2. Captura el resultado del experimento anterior. Lo que ves ¿Cómo está relacionado con esta línea de código?

<img width="999" height="728" alt="Captura de pantalla 2025-09-17 150130" src="https://github.com/user-attachments/assets/0a1c1046-c37a-44ec-bb20-af71093829e0" />

**R//** El monitor serial muestra símbolos raros porque los datos van en binario.  
`struct.pack` empaqueta los valores en bytes: eficiente para enviar, pero ilegible para leer a simple vista.

### 3. ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII?
**R//** El formato binario tiene la ventaja de ser más compacto y eficiente: cada número ocupa un espacio fijo y exacto (ej. 2 bytes para un entero corto), mientras que en ASCII el mismo número podría usar varios caracteres. La desventaja es que en binario el mensaje no es legible sin decodificación: necesitamos conocer el formato ('>2h2B') y usar struct.unpack o algo equivalente para interpretarlo.
En cambio, en ASCII todo es legible a simple vista, pero ocupa mucho más espacio y hace la transmisión más lenta.

### 4. Captura el resultado del experimento. ¿Cuántos bytes se están enviando por mensaje? ¿Cómo se relaciona esto con el formato '>2h2B'? ¿Qué significa cada uno de los bytes que se envían?
**R//** Cada mensaje ocupa 6 bytes. Esto se deduce directamente del formato '>2h2B':
```
> → orden big-endian.
2h → dos enteros cortos, 2 bytes cada uno = 4 bytes.
2B → dos enteros sin signo de 1 byte cada uno = 2 bytes.
Total = 4 + 2 = 6 bytes.
```
Cada byte tiene un propósito definido:
- Bytes 1–2: valor de xValue.
- Bytes 3–4: valor de yValue
- Byte 5: estado del botón A (0 = libre, 1 = presionado).
- Byte 6: estado del botón B (0 = libre, 1 = presionado).

### 5. ¿Cómo se verían esos números en el formato '>2h2B'? (Números Positivos y Negativos para valores de "xValue" y "yValue")
**R//** En el formato `'>2h2B'`, los valores `xValue` y `yValue` se representan como enteros de 16 bits en big-endian.  
  - Si el número es **positivo**, esos 2 bytes empiezan con `00`.  
  - Si el número es **negativo**, esos 2 bytes empiezan con `FF` (porque se usan números en complemento a dos).  
- Los botones (`aState` y `bState`) ocupan **1 byte cada uno**:  
  - `00` significa que el botón no está presionado.  
  - `01` significa que el botón está presionado.

### 6. Captura el resultado del experimento. ¿Qué diferencias ves entre los datos en ASCII y en binario? ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII? ¿Qué ventajas y desventajas ves en usar un formato ASCII en lugar de binario?

<img width="996" height="726" alt="Captura de pantalla 2025-09-17 151250" src="https://github.com/user-attachments/assets/d6523df6-9570-4f69-a4d6-14f0ee4286be" />

**R//** Cuando los datos se mandan en **ASCII**, se pueden leer fácilmente porque salen en texto claro, como los números separados por comas. Eso me sirve mucho para probar y entender qué está pasando. El problema es que ocupa más espacio y la comunicación se vuelve un poco más lenta.  

En cambio, en **binario** los datos viajan como bytes compactos. Eso hace que todo sea mucho más rápido y eficiente, pero ya no puedo leerlos directamente en el monitor serial porque se ven símbolos raros. Para entenderlos necesito un programa que los interprete.

---

# Actividad 03:

### 1. Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y ahora no es necesario.
**R//** Antes, cuando los datos iban en texto (ASCII), tocaba poner comas como separadores y un `\n` al final para saber dónde terminaba cada paquete. Ahora eso ya no hace falta porque los datos van en binario con un tamaño fijo. Eso significa que siempre sé que un paquete ocupa 6 bytes, entonces no necesito separadores, solo leerlos directo.

### 2. Compara el código de la unidad anterior relacionado con la recepción de los datos seriales que ves ahora. ¿Qué cambios observas?  
**R//**  El cambio más grande es que dejamos de trabajar con texto y pasamos a trabajar con datos binarios. Antes tenía que dividir la cadena en comas y convertir cada valor. Ahora simplemente leo los 6 bytes y sé que ahí vienen `xValue`, `yValue`, y los estados de los botones. Es más rápido, más claro y me evito errores de separar cadenas.

### 3. ¿Qué ves en la consola? ¿Por qué crees que se produce este error?  
**R//** En la consola a veces aparecen valores que no tienen sentido, como si `xValue` fuera un número muy raro. Eso pasa porque el programa puede empezar a leer en la mitad de un paquete. Como ya no hay delimitadores como `\n`, si se desacomoda la lectura se rompe la alineación y se ven datos incorrectos.

### 4. Analiza el código, observa los cambios. Ejecuta y luego observa la consola. ¿Qué ves?  
**R//** El nuevo código manda un byte de inicio (0xAA), después los 6 bytes con los datos, y al final un checksum para verificar que todo llegó bien. En p5.js, el código ahora busca ese byte de inicio y revisa si el paquete es válido. En la consola puedo ver que ya no se generan errores tan fáciles, porque el programa solo procesa paquetes completos y correctos.

### 5. Analiza el código, observa los cambios. Ejecuta y luego observa la consola. ¿Qué ves?  
**R//** Cuando corro el programa y conecto el micro:bit, veo que empieza a dibujar líneas que giran como agujas de reloj. Puedo cambiar su tamaño, color y forma con algunas teclas del teclado. Al principio no respondía a los botones del micro:bit, pero después de los cambios ya se puede usar el botón A para girar las líneas y el botón B para cambiar su color.

### 6. ¿Qué cambios tienen los programas y qué puedes observar en la consola del editor de p5.js?
**R//** El programa ahora combina las teclas con los botones del micro:bit. Con el botón A hago que las líneas giren, con el botón B cambio el color y también el tamaño dependiendo del color. Las teclas del teclado siguen funcionando para otras funciones. En la consola se nota que los datos llegan ordenados y que ya no se producen errores de sincronización como antes.

---

# Fase Reflect:

## Comparación entre protocolo ASCII y binario

En la unidad pasada usé un protocolo en ASCII, y en esta trabajé con binario. Según lo que experimenté en las aplicaciones, esta es la comparación:

| Aspecto       | ASCII (unidad anterior)                                   | Binario (unidad actual)                                                        |
|---------------|-----------------------------------------------------------|--------------------------------------------------------------------------------|
| **Eficiencia** | Poco eficiente porque cada número ocupa varios caracteres. | Más eficiente, ya que todo se envía en bytes compactos.                         |
| **Velocidad**  | Más lento, al ser más datos por cada valor.                | Más rápido porque se transmiten menos bytes por mensaje.                        |
| **Facilidad**  | Muy fácil de leer y depurar en el monitor serial.          | Difícil de interpretar directamente, necesito código para entenderlo.           |
| **Recursos**   | Usa más recursos de transmisión.                           | Menos recursos de transmisión y más óptimo para proyectos reales.               |

**Ejemplo concreto:**  
En ASCII, un valor como `-123` ocupa 4 caracteres (`-`, `1`, `2`, `3`). En binario, ese mismo valor cabe en **2 bytes** (`Int16`). Se nota de inmediato la diferencia de espacio y velocidad.

## ¿Por qué fue necesario introducir framing en el protocolo binario?
**R//** Porque al enviar solo bytes sueltos, no hay manera de saber dónde empieza y termina un paquete. El framing sirve para darle estructura a los datos y poder detectar mensajes completos.

## ¿Cómo funciona el framing?
**R//** Se añaden **marcadores especiales** (como un byte de inicio fijo y un tamaño definido) que permiten identificar dónde empieza y dónde termina un paquete. Así, aunque lleguen datos mezclados o incompletos, puedo separarlos correctamente.

## ¿Qué es un carácter de sincronización?
**R//** Es un byte único que marca el **inicio del paquete**. Sirve para alinear el receptor con el emisor. En mi caso, fue `0xaa`.

## ¿Qué es el checksum y para qué sirve?
**R//** Es un valor calculado a partir de los datos que viajan en el paquete. Sirve para verificar si los datos llegaron completos y sin errores. Si el checksum calculado no coincide con el que vino en el mensaje, sé que el paquete está corrupto.

## En la función `readSerialData()`

### ¿Qué hace la función concat? ¿Por qué?
**R//** La función `concat` junta los nuevos datos que llegan con los que ya estaban en el buffer. Así voy acumulando los bytes hasta poder armar un paquete completo.

```js
serialBuffer = serialBuffer.concat(newData);
```

### ¿Por qué el bucle recorre solo si hay 8 o más bytes?
**R//** Porque cada paquete definido en este protocolo tiene exactamente 8 bytes. Si hay menos, no puedo interpretarlo completo.

### ¿Qué significa 0xaa?
**R//** Es el byte especial de sincronización. Siempre está al inicio de un paquete y me ayuda a distinguirlo de cualquier otro dato basura.

### ¿Qué hace shift y la instrucción continue? ¿Por qué?
**R//** 
- `shift()` elimina el primer byte del buffer.  
- `continue` hace que el bucle pase directo a la siguiente iteración. 

Esto se usa cuando el primer byte no es 0xaa. Básicamente voy sacando datos hasta encontrar el inicio correcto del paquete.

### ¿Qué hace la instrucción break si hay menos de 8 bytes?
**R//** El break sale del bucle porque no hay suficientes datos para armar un paquete completo. Evita procesar información incompleta.

## Slice vs Splice:
- `slice(0,8)` → copia los primeros 8 bytes (para procesarlos).  
- `splice(0,8)` → elimina esos 8 bytes del buffer (para que no se vuelvan a usar).

Primero se copia el paquete, y luego se elimina del buffer. Así puedo seguir procesando los nuevos que lleguen.

### ¿Cómo opera la función reduce?
**R//** La función reduce recorre el arreglo de bytes y acumula la suma de todos. Después se hace % 256 para que el resultado quede dentro de un byte (0–255). Es básicamente cómo se calcula el checksum.

### ¿Por qué se compara el checksum enviado con el calculado?
**R//** Para comprobar que los datos no se dañaron en el camino.
- Si son iguales → el paquete es válido.
- Si no → se descarta, porque hubo un error en la transmisión.

### ¿Qué hace la instrucción continue en ese código?
**R//** Hace que el programa salte ese paquete defectuoso y siga con el siguiente. Así no se detiene toda la comunicación solo por un error.

### ¿Qué es un DataView? ¿Para qué se usa?
**R//** DataView es un objeto que me deja interpretar los bytes crudos de un buffer como distintos tipos de datos (enteros con signo, sin signo, etc.). Es muy útil cuando tengo datos binarios.

### ¿Por qué es necesario hacer estas conversiones?
**R//** Porque los valores llegan como una **secuencia de bytes** y no como números listos.  
Con `DataView` puedo reconstruirlos correctamente:

- `getInt16(0)` → toma 2 bytes y los interpreta como un número con signo (**xValue**).  
- `getInt16(2)` → lo mismo, pero para **yValue**.  
- `getUint8(4)` y `getUint8(5)` → interpretan 1 byte cada uno como valores de los botones.  

Si intentara leer el buffer directamente, no tendría la separación ni el formato correcto de los datos.



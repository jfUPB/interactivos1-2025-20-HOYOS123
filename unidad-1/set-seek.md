# Unidad 1

## 🔎 Fase: Set + Seek

# ACTIVIDAD 1

## ¿Qué es un sistema físico interactivo?

**R//** Un sistema físico interactivo es un conjunto de elementos físicos y digitales que perciben el entorno mediante sensores, procesan esa información y responden con acciones. Estos combinan lo físico (como sensores o motores) con lo digital (como un programa o computadora) para dar respuesta.

## ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

**R//** Saber sobre sistemas físicos interactivos es útil en muchos trabajos porque combina la parte física (como sensores o motores) con la parte digital (como programas o computadoras). Esto permite crear cosas que reaccionan al entorno, como un robot o un dispositivo médico. También ayuda a crear productos que no solo se ven bien, sino que también responden al entorno o al usuario. Por ejemplo, se pueden diseñar objetos o espacios que reaccionen al tacto, al movimiento o a la luz. Estas experiencias innovadoras son útiles e interactivas, para aplicarlas ya sea en diseño industrial, de producto o de experiencias.

---

# ACTIVIDAD 2

## ¿Qué es el diseño/arte generativo?

**R//** Es una forma de crear donde, en lugar de hacer todo manualmente, se dan reglas, se usan algoritmos o sistemas automáticos para generar imágenes, formas o diseños. Es como poner las instrucciones y dejar que la computadora o el proceso cree cosas aleatorias, mezclando creatividad humana con la capacidad de la máquina para experimentar.

## ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?

**R//** Este permite usar la tecnología para crear proyectos más innovadores y únicos, combinando creatividad con automatización. Esto ayuda a desarrollar desde gráficos, productos, instalaciones artísticas o experiencias digitales que son dinámicas y que pueden cambiar o adaptarse solas. Además, demuestra saber trabajar con herramientas digitales avanzadas y pensar de forma creativa para resolver problemas o sorprender a las personas.

---

# ACTIVIDAD 3

## ¿Cuáles son los Inputs, Outputs y el proceso?

**R//** Los Inputs son los botones A y B. El puerto USB de la torre donde se conecta el cable que le da señal al Micro:Bit, el acelerómetro. Los Outputs son el Display, los datos de la computadora...
En el proceso debes conectar el micro:bit al computador, Cargarlo al programa p5.js, la librería de comunicación serial debes añadirla, después se copia el código, a continuación ejecutas el sketch, conectas el micro:bit desde el navegador y prueba los botones y sensores para ver cómo reacciona la animación cuando pulsas "Send Love".

---

# ACTIVIDAD 4

## Mi propio programa en p5.js:

#### Enlace del programa:
[Enlace p5.js Editor](https://editor.p5js.org/HOYOS123/sketches/YGns7BqpX)

#### Código del programa:
```javascript
function setup() {
  createCanvas(600, 600);
  noLoop(); // Solo dibuja una vez
  background(0);
  
  for (let i = 0; i < 200; i++) {
    let x = random(width);
    let y = random(height);
    let r = sin(x * 0.01) * 100 + 100;
    let g = cos(y * 0.01) * 100 + 100;
    let b = random(100, 255);
    fill(r, g, b, 150);
    noStroke();
    ellipse(x, y, random(10, 40));
  }
}
```

#### Captura de pantalla del resultado:
<img width="1919" height="988" alt="Captura de pantalla 2025-07-25 144725" src="https://github.com/user-attachments/assets/d35e4f8c-b25e-4caa-ae90-584193891be4" />

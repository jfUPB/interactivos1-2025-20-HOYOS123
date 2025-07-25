# Unidad 1

## 🤔 Fase: Reflect

# Parte 1: Recuperación de Conocimiento (Retrieval Practice)

1. **Características de un sistema físico interactivo**  
   Basándote en los ejemplos que vimos de sistemas físicos interactivos al iniciar el curso, describe las tres características que definen a un sistema físico interactivo.

**R//**
   - **Interactividad**: Es que el sistema responde a acciones del usuario o del entorno.  
   - **Procesamiento de la CPU**: existe una unidad que interpreta y actúa sobre la información recibida en el procesador.  
   - **Detección de inputs/outputs**: el sistema detecta señales del entorno (inputs) y genera respuestas (outputs) a través de pantallas, sonidos, etc.

2. **Modelo Input-Process-Output de Patrick Hübner**  
   Explica el modelo input-process-output de Patrick Hübner usando como ejemplo el sistema que construiste con micro:bit y p5.js en la Actividad 06.  
**R//** En la Actividad 06, construí un sistema físico interactivo usando un micro:bit y p5.js. Según el modelo de Patrick Hübner:
   - **Input**: Fue la acción física sobre el micro:bit, por ejemplo, presionar los botones A y B o sacudir el dispositivo.
   - **Proceso**: El micro:bit detectó esa acción y envió un mensaje serial a la computadora. Luego, en p5.js, el programa recibió ese mensaje y lo interpretó.
   - **Output**: Fue el efecto visual en el canvas de p5.js, como el cambio de color, que dependía de la acción ejecutada.


3. **Comunicación micro:bit y p5.js**  
   ¿Cuál es la función de la línea `uart.write('A')` en el código del micro:bit y qué función en p5.js se encarga de “escuchar” ese mensaje?
**R//** Esta línea en el código del micro:bit hace la función de enviar un carácter (en este caso, la letra `'A'`) por el puerto serial. Este mensaje actúa como una señal que indica que ocurrió un evento específico, como presionar un botón o sacudir el dispositivo. El programa p5.js la recibe para ejecutar una acción determinada.

4. **Arte/diseño tradicional vs generativo**  
   ¿Cuál es la diferencia fundamental entre el arte/diseño tradicional y el arte/diseño generativo?
**R//** La diferencia entre el arte/diseño tradicional y el arte/diseño generativo es que el primero se crea manualmente, con decisiones controladas directamente por el artista o diseñador, mientras que el generativo se produce mediante algoritmos que generan resultados variables, a partir de reglas programadas.

5. **Sacudir micro:bit para cambiar color en p5.js**  
   Imagina que quieres que un círculo en p5.js cambie a un color aleatorio cada vez que sacudes el micro:bit.  
   - Describe qué tendrías que programar en el micro:bit.  **R//** Debo hacer que al sacudir el micro:bit se detecte el movimiento utilizando el acelerómetro para que este mande un serial en bucle y cambie de color automáticamente.
   - Describe qué tendrías que programar en p5.js para lograrlo.  **R//** Necesito usar `serial.read()` para recibir el mensaje y al detectar ese valor, generar un nuevo color aleatorio para redibujar el círculo con ese nuevo color.

---

# Parte 2: Reflexión sobre tu proceso (Metacognición)

1. **Desafíos personales**  
   ¿Qué fue más desafiante para ti en esta unidad: la parte conceptual (entender qué es un sistema físico interactivo) o la parte técnica (hacer que el micro:bit y p5.js se comunicaran)? ¿Por qué?
**R//**

3. **Momento “¡Aha!”**  
   Describe el momento “¡Aha!” que tuviste cuando lograste que una acción en el micro:bit (presionar un botón, sacudirlo) tuviera un efecto visible en el canvas de p5.js por primera vez.  
   - ¿Qué fue lo que entendiste en ese instante?
**R//**

4. **Utilidad del curso**  
   Al inicio de la unidad te pregunté: “¿Este curso para qué me sirve?”.  
   - Después de experimentar y construir tu primer prototipo, ¿cómo ha cambiado o se ha vuelto más concreta tu respuesta a esa pregunta?
**R//**

5. **Método paso a paso**  
   El tutorial de la Actividad 05 te llevó paso a paso.  
   - ¿Cómo te sentiste con ese método de aprendizaje?  **R//**
   - ¿Te dio seguridad o preferirías haberlo intentado por tu cuenta desde el principio?  **R//**

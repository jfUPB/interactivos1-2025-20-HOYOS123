# Unidad 1

## 🤔 Fase: Reflect

### Actividad 7
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

**R//** Lo más desafiante fue la parte técnica, especialmente lograr que la comunicación entre el micro:bit y p5.js funcionara correctamente.

3. **Momento “¡Aha!”**  
   Describe el momento “¡Aha!” que tuviste cuando lograste que una acción en el micro:bit (presionar un botón, sacudirlo) tuviera un efecto visible en el canvas de p5.js por primera vez.  
   - ¿Qué fue lo que entendiste en ese instante?

**R//** Cuando presioné el botón del micro:bit y vi que el círculo cambiaba en la pantalla. Ahí entendí cómo un dispositivo físico puede controlar lo que pasa en una visualización digital.

4. **Utilidad del curso**  
   Al inicio de la unidad te pregunté: “¿Este curso para qué me sirve?”.  
   - Después de experimentar y construir tu primer prototipo, ¿cómo ha cambiado o se ha vuelto más concreta tu respuesta a esa pregunta?

**R//** Antes no tenía ni idea de lo que trataría este curso. Ahora veo que este curso me sirve para crear experiencias interactivas reales que pueden aplicarse en arte digital, educación, prototipado y diseño creativo.

5. **Método paso a paso**  
   El tutorial de la Actividad 05 te llevó paso a paso.  
   - ¿Cómo te sentiste con ese método de aprendizaje?
   - ¿Te dio seguridad o preferirías haberlo intentado por tu cuenta desde el principio?
  
**R//** Me dio seguridad porque me ayudó a entender cada parte del proceso sin perderme. Ya con eso claro, pude comenzar a experimentar con más confianza.


### Actividad 8






### Actividad 9
## Continuar:
**R//** El video del Mario Kart fue el que más me inspiró en esta unidad. Me ayudó a visualizar cómo los sistemas físicos interactivos pueden transformar una experiencia digital en algo mucho más inmersivo.
Ver cómo los movimientos del cuerpo, sensores y objetos físicos se integraban con el juego me hizo entender el verdadero potencial de estos sistemas para crear nuevas formas de interacción. Me motivó a pensar en cómo podría usar sensores del micro:bit para controlar visualizaciones o experiencias interactivas propias, incluso más allá del aula, en proyectos creativos personales.

## Dejar de hacer
**R//** En algunos momentos la explicación técnica fue un poco rápida, especialmente al momento de conectar p5.js con micro:bit. Me costó un poco seguir la lógica del paso a paso sin ejemplos más guiados. Tal vez se podría incluir un repaso más visual o pausado de esa conexión, o ejemplos más simples al inicio para no perderse.

## Empezar a hacer
**R//** Me genera mucha curiosidad experimentar con los otros sensores del micro:bit, como el de luz o temperatura. Me gustaría crear visualizaciones más complejas en p5.js, como representaciones visuales que reaccionen al entorno en tiempo real, mezclando arte y datos. También me interesaría ver ejemplos de proyectos artísticos más ambiciosos hechos con estas herramientas.

## Balance inspiración vs. técnica
**R//** Me pareció que hubo un buen equilibrio entre la inspiración y la técnica. Los videos de la Actividad 01 me ayudaron a imaginar lo que se puede lograr, y luego las actividades técnicas me mostraron cómo empezar a hacerlo. Sin embargo, al principio puede sentirse un poco abrupto el salto de la inspiración al código. Tal vez podría haber una actividad intermedia que combine ambas cosas.

## Comentario adicional
**R//** En general, me gustó mucho la unidad. Me hizo ver que es posible unir lo creativo con lo tecnológico de una manera divertida y significativa. Me gustaría que en las próximas unidades haya más espacio para experimentar libremente con lo aprendido, y quizás trabajar en mini proyectos personales desde el inicio.

# Unidad 3

## 🤔 Fase: Reflect

### Parte 1: recuperación de conocimiento (Retrieval Practice)

#### Describe con tus palabras qué es una máquina de estados. ¿Cuáles son sus cuatro componentes fundamentales que has utilizado en esta unidad?
**R//** Una máquina de estados es como una forma de organizar un programa en “situaciones” más claros. Cada estado representa lo que el sistema está haciendo en ese momento, y dependiendo de lo que pase (un evento, una acción del usuario, un cambio en el tiempo), la máquina puede pasar a otro estado.

Los cuatro componentes que utilicé en eta unidad fueron:

#### Estados definidos
- CONFIG → cuando se ajusta el tiempo de la bomba.
- ARMED → cuando ya está activada y comienza la cuenta regresiva.
- EXPLODED → cuando el tiempo llega a cero y la bomba explota.

#### Eventos de entrada:
- Botones A y B. (A Aumenta el tiempo de uno en uno, y B disminuye el tiempo de uno en uno). 
- Botones S y R. (El botón S significa "Shake" ---> "Agitar" y el botón R significa "Restaurar").
- La secuencia (A, B, B, A). Es el código que desactiva la bomba.

#### Transiciones:
Son las reglas que indican cómo se pasa de un estado a otro (por ejemplo, de CONFIG a ARMED al agitar, o de ARMED a EXPLODED cuando el contador llega a 0).

#### Acciones:
- lo que hace el sistema como respuesta (sumar/restar tiempo, iniciar cuenta regresiva, mostrar explosión, reiniciar, etc.).
---------------------------------------------
#### Explica por qué la técnica de máquina de estados es tan útil para gestionar la “concurrencia” (atender varios eventos y tareas “al mismo tiempo”) en un dispositivo con un solo hilo de ejecución como el micro:bit o en p5.js. ¿Qué problema soluciona en comparación con usar funciones como sleep()?
**R//** La técnica de máquina de estados es muy útil para mí porque permite que organice bien un programa, ajustando su comportamiento en estados claros y que vaya reaccionando a los eventos que ocurren sin detenerse nunca.

Los dispositivos micro:bit y p5.js son con un solo hilo de ejecución, Esto significa que el programa no puede hacer dos o más cosas al mismo tiempo. La máquina de estados resuelve eso al simular **concurrencia** pero solo se ejecuta una instrucción a la vez, el programa revisa constantemente en qué estado está y qué evento debe atender, dando la sensación de que gestiona varias tareas al mismo tiempo.

Esto soluciona el gran problema de usar la función sleep(), ya que esta hace que se pause completamente el programa y también bloquea que el sistema atienda otros eventos. En cambio, con una máquina de estados el código nunca se detiene; solo se actualiza el estado en cada ciclo y se reacciona a lo que pase, manteniendo el programa en funcionamiento.

---------------------------------------------
#### Imagina que tienes que añadir una nueva funcionalidad a la bomba: si se recibe un evento especial (por ejemplo, una combinación de botones o un comando serial) mientras la cuenta regresiva está activa, el tiempo se reduce a la mitad. ¿Cómo modificarías tu diagrama de máquina de estados para incluir este nuevo evento y acción?
**R//** Si quiero añadir una funcionalidad más, no necesito crear un estado nuevo: la bomba puede seguir estando en "ARMED" (Conteo regresivo), pero si puedo crear un evento más que ocurra dentro de este estado.

Por ejemplo, cuando llego a ese evento la máquina no cambia de estado, pero ejecuta una acción interna... puede ser, dividir el tiempo restante por la mitad. Es decir, sigo en el mismo estado "ARMED" pero con un cambio en la variable de tiempo.

--------------------------------------------
#### Explica qué es un “vector de prueba” y por qué es una herramienta crucial para verificar que una máquina de estados funciona como se espera.

--------------------------------------------
### Parte 2: reflexión sobre tu proceso (Metacognición)
#### ¿Qué parte del diseño de la bomba te resultó más desafiante: crear el diagrama de estados o traducir ese diagrama a código? ¿Por qué?

--------------------------------------------
#### Describe un error o “bug” que encontraste al implementar tu programa. ¿Cómo te ayudó pensar en términos de estados, eventos y transiciones a identificar y solucionar el problema?

--------------------------------------------
#### El problema de la bomba era complejo. ¿Qué estrategia usaste para abordarlo? ¿Comenzaste con una versión simple y añadiste funcionalidades poco a poco?

--------------------------------------------
#### Ahora que entiendes el patrón de máquina de estados, ¿En qué otro tipo de proyecto o sistema de entretenimiento digital crees que podrías aplicarlo?

--------------------------------------------



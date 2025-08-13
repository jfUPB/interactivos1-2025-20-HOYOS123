# Unidad 2

## 🤔 Fase: Reflect

# Bitácora – Máquina de Estados

## Parte 1 – Recuperación de conocimiento (Retrieval Practice)

1. **Describe con tus palabras qué es una máquina de estados. ¿Cuáles son sus cuatro componentes fundamentales que has utilizado en esta unidad?**  

**R//** Una máquina de estados organiza un programa en estados que reaccionan a eventos y cambian o ejecutan acciones según ciertas condiciones.
   
   Componentes clave: **estados, eventos, condiciones y acciones**.

3. **Explica por qué la técnica de máquina de estados es tan útil para gestionar la “concurrencia” (atender un temporizador y botones “al mismo tiempo”) en un dispositivo con un solo hilo de ejecución como el micro:bit. ¿Qué problema soluciona en comparación con usar funciones como sleep()?**

**R//** Permite manejar varias tareas “a la vez” en un solo hilo sin bloquear el programa con `sleep()`. Revisa continuamente botones, temporizador y sensores en ciclos rápidos.

4. **Imagina que tienes que añadir una nueva funcionalidad a la bomba temporizada: si se agita (shake) el micro:bit mientras la cuenta regresiva está activa, el tiempo se reduce a la mitad. ¿Cómo modificarías tu diagrama de máquina de estados para incluir este nuevo evento y acción?**  

**R//** En el estado de cuenta regresiva, agregar una transición interna:  
   - Evento: `shake`  
   - Acción: tiempo = tiempo / 2

5. **Explica qué es un “vector de prueba” y por qué es una herramienta crucial para verificar que una máquina de estados funciona como se espera.**  

**R//** Lista de entradas y salidas esperadas que confirma que todas las transiciones y respuestas funcionan antes de probar en el micro:bit.

## Parte 2 – Reflexión sobre tu proceso (Metacognición)

1. **¿Qué parte del diseño de la bomba temporizada te resultó más desafiante: crear el diagrama de estados (Actividad 04) o traducir ese diagrama a código MicroPython (Actividad 05)? ¿Por qué?**  

**R//** Hacer el diagrama fue complicado, pero también pasar del diagrama al código fue lo más difícil.

2. **Describe un error o “bug” que encontraste al implementar tu programa. ¿Cómo te ayudó pensar en términos de estados, eventos y transiciones a identificar y solucionar el problema?**  

**R//** El temporizador no se reiniciaba al pasar de configuración a cuenta regresiva. La acción estaba en el lugar equivocado.

3. **El problema de la bomba era complejo. ¿Qué estrategia usaste para abordarlo? ¿Comenzaste con una versión simple y añadiste funcionalidades poco a poco?**  

**R//** Comencé con algo básico y fui añadiendo funciones poco a poco.

4. **Ahora que entiendes el patrón de máquina de estados, ¿En qué otro tipo de proyecto o sistema de entretenimiento digital crees que podrías aplicarlo?**  

**R//** En un juego simple donde el personaje tenga estados como caminar, atacar o estar herido.

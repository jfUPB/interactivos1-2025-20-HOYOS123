# Unidad 2

## 🔎 Fase: Set + Seek

# 📘 Bitácora de Actividades

## Actividad 1

### ¿Cómo funciona este ejemplo?

Este programa utiliza una clase llamada `Pixel` para controlar el encendido y apagado de LEDs en el display del micro:bit. Cada objeto `Pixel` tiene un estado interno que cambia en función del tiempo transcurrido, alternando entre encendido (valor 9) y apagado (valor 0). Se actualiza constantemente dentro de un bucle infinito (`while True`), lo que permite que los píxeles parpadeen.

### Estados en el programa

- El estado "Init" es el inicial, este configura el tiempo de inicio y muestra el píxel por primera vez.
- El estado en espera es el "WaitTimeout", en este se verifica si ha pasado el intervalo de tiempo para alternar el estado del LED.

### Eventos / Inputs en el programa

El script no depende de un imput ya que sus eventos son internos.

- Paso del tiempo medido con `utime.ticks_diff(...)`.
- El ciclo `while True` que permite ejecutar constantemente `update()`.

### Acciones en el programa

Las acciones que realiza cada objeto Pixel estàn definidas dentro del mètodo update()

- Asignar el tiempo actual a `startTime`.
- Cambiar `pixelState` entre 0 (apagado) y 9 (encendido).
- Usar `display.set_pixel(x, y, brightness)` para mostrar el estado actual del LED.

---

## Actividad 2: Implementando un semáforo con máquina de estados

### Descripción

Se implementa un semáforo con tres estados: rojo, amarillo y verde. Cada color está representado por un LED en el display del micro:bit. El programa cambia de estado según el tiempo transcurrido, simulando el comportamiento de un semáforo real.

### Código

```python
from microbit import *
import utime

class Semaforo:
    def __init__(self):
        self.state = "ROJO"
        self.startTime = utime.ticks_ms()
    
    def update(self):
        now = utime.ticks_ms()
        elapsed = utime.ticks_diff(now, self.startTime)
        
        if self.state == "ROJO":
            self.encender(0, 0)  # LED rojo
            if elapsed > 3000:
                self.state = "VERDE"
                self.startTime = now

        elif self.state == "VERDE":
            self.encender(0, 2)  # LED verde
            if elapsed > 3000:
                self.state = "AMARILLO"
                self.startTime = now

        elif self.state == "AMARILLO":
            self.encender(0, 1)  # LED amarillo
            if elapsed > 1000:
                self.state = "ROJO"
                self.startTime = now

    def encender(self, x, y):
        display.clear()
        display.set_pixel(x, y, 9)

semaforo = Semaforo()

while True:
    semaforo.update()
```

## Actividad 3: 

### ¿Por qué este programa permite realizar varias tareas a la vez?

El programa está diseñado con una máquina de estados que revisa constantemente dos cosas: 
si ha pasado cierto tiempo para cambiar la imagen y si se ha presionado el botón A. Aunque el micro:bit solo hace una cosa por vez,
eso permite que el sistema parezca hacer ambas cosas al mismo tiempo: seguir una secuencia automática y responder rápido al usuario.

### Estados, eventos y acciones

- **Estados:** INIT, HAPPY, SMILE y SAD.  
- **Eventos:** Presionar el botón A o que pase el tiempo definido para cada estado.  
- **Acciones:** Mostrar una imagen, reiniciar el temporizador y cambiar de estado.

### Vectores de prueba según como lo describe la máquina de estados

#### Prueba 1  
- **Estado:** HAPPY  
- **Evento:** botón A  
- **Resultado esperado:** pasa a SAD  
- Funciona correctamente

#### Prueba 2  
- **Estado:** SMILE  
- **Evento:** botón A  
- **Resultado esperado:** pasa a HAPPY  
- Funciona correctamente

#### Prueba 3  
- **Estado:** SAD  
- **Evento:** pasa el tiempo  
- **Resultado esperado:** pasa a HAPPY  
- Funciona correctamente

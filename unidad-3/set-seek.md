# Unidad 3

## 🔎 Fase: Set + Seek

# Bomba 3.0
### Máquina de estados controlada por EVENTOS genéricos desde:
- Botones/gestos del micro:bit
- Puerto serial (p5.js, terminal, etc.)

from microbit import *
import utime

display.clear()

---------------------------
# Event bus (única fuente de verdad)
class Event:
    def __init__(self):
        self.value = 0  # 0 = sin evento

    def set(self, v):
        self.value = v  # 'A', 'B', 'S', 'T'

    def read(self):
        return self.value

    def clear(self):
        self.value = 0

# Instancia global de eventos (consumida por BombTask)
event = Event()

---------------------------
# Fuentes de eventos
class SerialTask:
    def __init__(self):
        # Ajusta baudrate si lo requieres
        uart.init(baudrate=115200)
---------------------------
    def update(self):
        # Lee de a 1 byte para eventos simples
        if uart.any():
            data = uart.read(1)
            if data:
                b = data[0]
                if b == ord('A'):
                    event.set('A')
                elif b == ord('B'):
                    event.set('B')
                elif b == ord('S'):
                    event.set('S')
                elif b == ord('T'):
                    event.set('T')

class ButtonTask:
    def __init__(self):
        pass

    def update(self):
        if button_a.was_pressed():
            event.set('A')
        elif button_b.was_pressed():
            event.set('B')
        elif accelerometer.was_gesture('shake'):
            event.set('S')
        elif pin_logo.is_touched():
            event.set('T')

---------------------------
# Máquina de estados de la bomba
class BombTask:
    def __init__(self):
        self.PASSWORD = ['A', 'B', 'A']
        self.key = [''] * len(self.PASSWORD)
        self.keyindex = 0

        self.count = 20  # valor inicial por defecto
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'

        display.clear()
        display.show(self.count, wait=False)

    def _reset_key(self):
        self.keyindex = 0
        for i in range(len(self.key)):
            self.key[i] = ''

    def _append_key(self, k):
        # Agrega A/B a la clave si hay espacio
        if self.keyindex < len(self.key):
            self.key[self.keyindex] = k
            self.keyindex += 1

    def _check_password(self):
        for i in range(len(self.key)):
            if self.key[i] != self.PASSWORD[i]:
                return False
        return True

    def update(self):
        now_event = event.read()

        if self.state == 'CONFIG':
            # Aumentar tiempo
            if now_event == 'A':
                event.clear()
                self.count = min(self.count + 1, 60)
                display.show(self.count, wait=False)

            # Disminuir tiempo
            elif now_event == 'B':
                event.clear()
                self.count = max(10, self.count - 1)
                display.show(self.count, wait=False)

            # Armar bomba
            elif now_event == 'S':
                event.clear()
                self._reset_key()
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            # Tick de 1 segundo
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) >= 1000:
                self.startTime = utime.ticks_ms()
                self.count -= 1
                display.show(self.count, wait=False)
                if self.count <= 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'
                    # No retornamos aún; dejamos consumir un posible 'T' en el próximo ciclo

            # Entrada de clave (A/B)
            if now_event == 'A':
                event.clear()
                self._append_key('A')

            elif now_event == 'B':
                event.clear()
                self._append_key('B')

            # ¿Se completó la clave?
            if self.keyindex == len(self.key) and self.state == 'ARMED':
                if self._check_password():
                    # Desactiva: vuelve a CONFIG y resetea contador
                    self.count = 20
                    display.show(self.count, wait=False)
                    self._reset_key()
                    self.state = 'CONFIG'
                else:
                    # Clave incorrecta: borra e intenta de nuevo
                    self._reset_key()

        elif self.state == 'EXPLODED':
            # Reinicio con 'T'
            if now_event == 'T':
                event.clear()
                self.count = 20
                display.show(self.count, wait=False)
                self.startTime = utime.ticks_ms()
                self._reset_key()
                self.state = 'CONFIG'
---------------------------
# Instanciación y bucle principal
serialTask = SerialTask()
buttonTask = ButtonTask()
bombTask = BombTask()

# Bucle cooperativo
while True:
    serialTask.update()
    buttonTask.update()
    bombTask.update()
    utime.sleep_ms(10)  # pequeño respiro al CPU
---------------------------

# Bitácora - Actividad 05

## Modelo de la bomba 3.0

La bomba funciona como una **máquina de estados** que tiene tres modos principales:

### 1. CONFIG  
En este estado la bomba está lista para configurarse.  
- Se puede aumentar el tiempo con `A`.  
- Disminuir el tiempo con `B`.  
- Armar la bomba con `S`.  

El tiempo no puede subir de 60 ni bajar de 10.

### 2. ARMED  
Aquí empieza la cuenta regresiva. Cada segundo baja el contador y también se puede introducir la clave (`A-B-A`).  
- Si la clave es correcta → se reinicia el contador a 20 y vuelve a **CONFIG**.  
- Si la clave es incorrecta → la bomba sigue en **ARMED**.  
- Si el contador llega a 0 → la bomba pasa a **EXPLODED**.

### 3. EXPLODED  
En este estado la bomba muestra una calavera.  
- Solo se puede reiniciar con el evento `T`, que resetea el contador a 20 y vuelve a **CONFIG**.

---

## Vectores de prueba

La siguiente tabla resume los vectores de prueba de la bomba 3.0:

| Estado inicial | Evento disparador | Acciones realizadas | Estado final |
|----------------|-------------------|---------------------|--------------|
| CONFIG         |       `A`        | Aumenta el tiempo (máx 60) | CONFIG |
| CONFIG | `B` | Disminuye el tiempo (mín 10) | CONFIG |
| CONFIG | `S` | Inicia la cuenta regresiva desde el tiempo configurado | ARMED |
| ARMED | Timer 1s | Resta 1 al contador | ARMED / EXPLODED (si llega a 0) |
| ARMED | `A` | Guarda un `A` en la clave | ARMED |
| ARMED | `B` | Guarda un `B` en la clave | ARMED |
| ARMED | Clave correcta `A-B-A` | Reinicia contador en 20 y regresa a config | CONFIG |
| ARMED | Clave incorrecta | Borra la clave y sigue igual | ARMED |
| EXPLODED | `T` | Reinicia contador (20) y limpia pantalla | CONFIG |

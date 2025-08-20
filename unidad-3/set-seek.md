# Unidad 3

## 🔎 Fase: Set + Seek

# Imports go at the top
from microbit import *
import utime

display.clear()


class Event:
def init(self):
self.value = 0

def set(self, _val):
self.value = _val

def clear(self):
self.value = 0

def read(self):
return self.value


class SerialTask:
def init(self):
uart.init(baudrate=115200)

def update(self):
if uart.any():
data = uart.read(1)
if data:
if data[0] == ord('A'):
event.set('A')
elif data[0] == ord('B'):
event.set('B')
elif data[0] == ord('S'):
event.set('S')
elif data[0] == ord('T'):
event.set('T')


class ButtonTask:
def init(self):
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


class BombTask:
def init(self):
self.PASSWORD = ['A', 'B', 'A']
self.key = [''] * len(self.PASSWORD)
self.keyindex = 0
self.count = 20
self.startTime = utime.ticks_ms()
self.state = 'CONFIG'
display.clear()
display.show(self.count, wait=False)

def update(self):
if self.state == 'CONFIG':
if event.read() == 'A':
event.clear()
self.count = min(self.count + 1, 60)
display.show(self.count, wait=False)

if event.read() == 'B':
event.clear()
self.count = max(10, self.count - 1)
display.show(self.count, wait=False)

if event.read() == 'S':
event.clear()
self.startTime = utime.ticks_ms()
self.state = 'ARMED'

elif self.state == 'ARMED':
if utime.ticks_diff(utime.ticks_ms(), self.startTime) > 1000:
self.startTime = utime.ticks_ms()
self.count = self.count - 1
display.show(self.count, wait=False)
if self.count == 0:
display.show(Image.SKULL)
self.state = 'EXPLODED'

if event.read() == 'A':
event.clear()
self.key[self.keyindex] = 'A'
self.keyindex += 1

if event.read() == 'B':
event.clear()
self.key[self.keyindex] = 'B'
self.keyindex += 1

if self.keyindex == len(self.key):
passIsOK = True
for i in range(len(self.key)):
if self.key[i] != self.PASSWORD[i]:
passIsOK = False
break
if passIsOK:
self.count = 20
display.show(self.count, wait=False)
self.keyindex = 0
self.state = 'CONFIG'
else:
self.keyindex = 0

elif self.state == 'EXPLODED':
if event.read() == 'T':
event.clear()
self.count = 20
display.show(self.count, wait=False)
self.startTime = utime.ticks_ms()
self.state = 'CONFIG'


# Inicialización de tareas
event = Event()
bombTask = BombTask()
serialTask = SerialTask()
buttonTask = ButtonTask()

# Bucle principal
while True:
serialTask.update()
buttonTask.update()
bombTask.update()

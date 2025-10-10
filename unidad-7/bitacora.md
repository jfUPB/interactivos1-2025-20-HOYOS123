# Evidencias de la unidad 7
---

## ACTIVIDAD 01: Conexión entre el celular y el computador

1. **¿Qué URL de Dev Tunnels obtuviste?**  
**R//** Obtuve esta URL: `https://juanjo-devtunnel.use2.devtunnels.ms/](https://pt3fpsgv-3000.use2.devtunnels.ms/`. A esta URL le agregue `/mobile/` al final, como me indicaban en los pasos del ejercicio.
 
2. **¿Por qué crees que necesitamos usar esta URL en lugar de http://localhost:3000 o la IP local de tu computador para que el celular se conecte?**  
**R//** Porque `localhost` solo funciona dentro del mismo equipo donde está corriendo el servidor. La URL de Dev Tunnels crea un enlace público que permite que el celular, acceda al servidor local a través de internet.  

3. **Describe brevemente qué hace npm install y npm start.**  
**R//** `npm install` descarga e instala todas las dependencias necesarias del proyecto, como **express** y **socket.io**, para que funcione correctamente.  
Y `npm start` ejecuta el servidor definido en el archivo `server.js`, iniciando la aplicación en el puerto 3000.  

4. **¿Qué mensajes observaste en la terminal del servidor al conectar el cliente de escritorio y el cliente móvil? ¿Eran diferentes los mensajes o identificadores?**  
**R//** Sí, aparecieron mensajes como:  
- *“New client connected”* cada vez que abría una de las aplicaciones.  
- *“Received message => …”* cuando se enviaban datos del celular al escritorio.  
- *“Client disconnected”* cuando cerraba alguna pestaña.  
Cada cliente tenía un identificador diferente, lo que ayudaba al servidor a distinguir entre el móvil y el escritorio.  

5. **Describe el comportamiento observado: ¿Funcionó la interacción? ¿Hubo algún retraso (latencia)?**  
**R//** Sí, la interacción funcionó correctamente. Al mover el dedo en la pantalla del celular, el círculo rojo en el navegador del computador seguía el movimiento en tiempo real. Solo se notaba un leve retraso muy pequeño, pero en general la conexión fue fluida y estable.

---

## ACTIVIDAD 02: Conectividad y comunicación entre dispositivos  

1. **¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente?**  
**R//** Es necesario porque permite que un servidor local (que normalmente solo se puede acceder desde el mismo computador) sea visible en Internet públicamente.  
Funciona como un “puente” que conecta una URL pública con el puerto local donde corre el servidor. Así, cuando el celular accede a la URL del túnel, la solicitud viaja por Internet hasta el servicio de Dev Tunnels, que luego la reenvía a `localhost:3000`. De esa forma, ambos dispositivos se comunican aunque estén en redes diferentes.  

2. **Describe la función de touchMoved() y por qué se usa la variable threshold en el cliente móvil.**  
**R//** La función `touchMoved()` detecta cuando el usuario mantiene un dedo sobre la pantalla y lo mueve. Al llamarlo cada vez, obtiene las coordenadas actuales del toque (`mouseX` y `mouseY`) y las envía al servidor para actualizar la posición del círculo en el escritorio.  
La variable `threshold` se usa para evitar que se envíen demasiados mensajes por movimientos mínimos o temblores del dedo. Solo cuando el cambio de posición supera ese umbral, se envía un nuevo mensaje, optimizando el rendimiento y reduciendo la latencia.  

3. **Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?**

**R//**

A) **Dev Tunnels:**  
  - *Ventajas:* Me permite acceder al servidor desde cualquier red (Wi-Fi o datos móviles) sin tanto enredo. Es seguro y fácil de usar.  
  - *Desventajas:* Depende de una conexión estable a Internet y de los servicios de Microsoft; si el túnel se cierra, el acceso se pierde.  

B) **IP local:**  
  - *Ventajas:* No necesita Internet, solo una red local; puede ser más rápido en entornos cerrados.  
  - *Desventajas:* Solo funciona si ambos dispositivos están en la misma red y puede fallar por firewalls o configuraciones del router.  

4. **📸 Capturas de pantalla**

**R//** Computador.

<img width="1459" height="910" alt="image" src="https://github.com/user-attachments/assets/d4c56122-4c7d-4ae5-a24b-5dffb074a74f" />

**R//** Celular.

<img width="738" height="1600" alt="image" src="https://github.com/user-attachments/assets/341bd57e-21ba-4701-bcde-26aed2a99546" />

**R//** Terminal.

<img width="1271" height="515" alt="image" src="https://github.com/user-attachments/assets/4bcfe9a4-a4e5-44c5-bb54-e95647158d2c" />

**Mira la demostración aquí:**  
[Video demostrativo](https://youtube.com/shorts/SgP_dZ62z74?feature=share)

---

## ACTIVIDAD 03: Análisis del servidor (server.js)

1. **¿Cuál es la función principal de `express.static('public')` en este servidor?**  
**R//** Sirve para que el servidor muestre automáticamente todos los archivos que están en la carpeta **public**, como los HTML, CSS o scripts.  
A diferencia del `app.get('/ruta', …)` que usábamos antes, aquí no hay que crear rutas manualmente, todo se sirve de una forma más simple y directa.  


2. **Explica detalladamente el flujo de un mensaje táctil:**  
**R//** Cuando muevo el dedo en la pantalla del celular, la función `touchMoved()` envía un mensaje con las coordenadas del movimiento.  
El servidor recibe ese mensaje con `socket.on('message')`, lo muestra en la consola y luego lo reenvía a los demás clientes con `socket.broadcast.emit`.  
El escritorio recibe ese mensaje y actualiza la posición del círculo en tiempo real.  
Se usa `socket.broadcast.emit` porque así el mensaje llega a todos menos al que lo envió, evitando que el celular reciba su propio mensaje.  


3. **Si conectaras dos computadores de escritorio y un móvil a este servidor, y movieras el dedo en el móvil, ¿Quién recibiría el mensaje retransmitido por el servidor? ¿Por qué?**  
**R//** Los dos computadores de escritorio recibirían el mensaje, porque el servidor lo envía a todos los demás clientes conectados, excepto al que lo envió (el celular).  


4. **¿Qué información útil te proporcionan los mensajes `console.log` en el servidor durante la ejecución?**  
**R//** Sirven para ver lo que está pasando en el servidor: cuándo se conecta o se desconecta un cliente, y qué mensajes se están enviando.  
De esa forma uno puede comprobar si todo está funcionando bien o si hay algún problema en la comunicación.

---









# Evidencias de la unidad 6
---

### Preguntas sobre la ejecución y observación del proyecto

1. **¿Qué ocurrió en la terminal cuando ejecutaste `npm install`?** **R//** Se instalaron 120 paquetes y se generó el reporte de auditoría. La terminal mostró también un aviso de actualización de `npm`. 

   **¿Cuál crees que es su propósito?** **R//** El proposito de esto era descargar e instalar las dependencias necesarias del proyecto.

2. **¿Qué mensaje específico apareció en la terminal después de ejecutar `npm start`?**  
   **¿Qué indica este mensaje?**
   **R//** Server is listening on http://localhost:3000... Indica que el servidor está activo y escuchando conexiones en ese puerto.

4. **Describe lo que ves inicialmente en `page1` y `page2` en tu navegador.**
   **R//** En ambas páginas se ve un círculo rojo, y estos dos círculos están conectados entre sí por una línea.

   la línea representa la relación o conexión entre las dos páginas (Que cumplen la función de Clientes) a través del servidor.
   lo que se ve en page1 es prácticamente lo mismo que en page2; ambas muestran los mismos elementos (dos círculos rojos y la línea que los une).
   
6. **¿Qué mensajes aparecieron en la terminal del servidor cuando abriste `page1` y `page2`?**
   **R//** Conectando con el servidor...

7. **Describe qué sucede en ambas páginas del navegador cuando mueves una de las ventanas.**  
   - ¿Cambia algo visualmente? **R//** El ejercicio usa sincronización de ventanas, entonces lo que hagas en page1 se refleja en page2 y viceversa. Ya sea mover la pestaña por toda la pantalla o juntar los círculos.
     
   - ¿Qué mensajes aparecen (si los hay) en la consola del navegador (usualmente accesible con `F12 -> Pestaña Consola`) y en la terminal del servidor? **R//** Aparecen mensajes con las actualizaciones de posición de la ventana y confirmaciones de sincronización.

# Bitácora sobre Internet
---

## 1. **¿Qué es Internet?**
**R//** En mi casa, me conecto a Internet usando **Wi-Fi**. Mi computadora se conecta de forma inalámbrica a un módem/router, que está conectado por un cable al proveedor de Internet. Esa conexión Wi-Fi es como mi "rampa" de acceso a esta gran red de carreteras.

Si la rampa se rompe, por ejemplo, si el Wi-Fi se cae o el cable del módem se desconecta, ya no puedo acceder a ninguna página web ni servicio en línea. Sería como si mi vehículo quedara atrapado sin poder salir a la carretera, sin acceso a los servidores que contienen la información que necesito.

---

## 2. Relaciones Cliente-Servidor en la vida diaria
**R//** Un ejemplo claro de la relación Cliente-Servidor fuera del mundo digital es cuando voy a un restaurante. Yo soy el cliente, pido comida al mesero (que podríamos considerarlo como un “cliente” del chef). El chef actúa como el servidor, quien recibe la orden y prepara la comida para entregarla al cliente.

Otro ejemplo puede ser una biblioteca física: yo como cliente pido un libro en el mostrador, y el bibliotecario es el servidor que busca y me entrega el libro solicitado.

---

## 3. Desglose de una URL

Mi sitio de ejemplo: `https://www.wikipedia.org/wiki/Internet` (No es mi favorito pero lo utilicé mucho en mis tiempos del colegio)

- **Protocolo:** `https://` (indica que la comunicación será segura)
- **Nombre de dominio:** `www.wikipedia.org` (es el “edificio” donde está el contenido)
- **Ruta:** `/wiki/Internet` (especifica la página exacta dentro del sitio)

Si solo escribo `www.wikipedia.org` sin ruta, el servidor me envía la página principal o “página por defecto” del sitio.

---

## 4. Comparación HTTP vs Protocolos Seriales

**Similitudes:**
- Ambos definen reglas para que dos partes puedan comunicarse correctamente.
- Requieren un emisor y un receptor que entiendan el mismo protocolo.
- La comunicación se inicia por una petición y recibe una respuesta.

**Diferencias clave:**
- HTTP es más estructurado y trabaja a gran escala en la web, mientras que los protocolos seriales son más simples y directos.
- HTTP incluye cabeceras, métodos, códigos de estado, y puede transportar diferentes tipos de datos.

**¿Por qué HTTP necesita ser más complejo?**
- Porque maneja muchas situaciones (errores, seguridad, tipos de archivos, etc...) y necesita garantizar una comunicación clara y funcional entre millones de dispositivos distintos.

---

## 5. Análisis de una página web simple (formulario de login)

- **HTML:**  
  Campos de texto para usuario y contraseña, botón de enviar.

- **CSS:**  
  Color del botón, tipo y tamaño de letra, espaciado entre elementos.

- **JavaScript:**  
  Validación de que los campos no estén vacíos antes de enviar, mostrar mensaje de “contraseña incorrecta” sin recargar la página.

---

## 6. Comparación entre `draw()` en p5.js y el modelo basado en eventos

En p5.js, el bucle `draw()` se ejecuta constantemente, lo que es útil para animaciones o gráficos que necesitan actualizarse todo el tiempo. Pero, en la web, JavaScript funciona de una forma diferente: en lugar de ejecutarse todo el tiempo, espera a que ocurran eventos (como un clic o un mensaje) y entonces ejecuta una función específica.

**Ventajas del modelo basado en eventos para una interfaz de usuario web:**
- Es mucho más eficiente porque solo se ejecuta código cuando es necesario.
- No consume tantos recursos del sistema.
- Permite que las interfaces sean más rápidas y reactivas para el usuario.

**¿Sería eficiente tener un `draw()` redibujando toda la página 60 veces por segundo si nada ha cambiado?**  
Definitivamente no. Sería un desperdicio de recursos y podría hacer que la página se sienta lenta o pesada. El modelo basado en eventos tiene mucho más sentido para interfaces web donde las cosas solo cambian cuando el usuario hace algo.

---

## 7. Uso de JavaScript en cliente y servidor

Creo que usar JavaScript tanto en el navegador como en el servidor es muy útil porque permite a los desarrolladores trabajar con un solo lenguaje en ambos lados. Eso hace que el desarrollo sea más rápido, que haya menos errores por diferencias entre lenguajes y que se pueda incluso reutilizar código.

Además, es más fácil para alguien que está aprendiendo, porque no tiene que cambiar de mentalidad entre cliente y servidor. Todo se puede manejar con JavaScript.

---

## 8. Comunicación HTTP vs WebSockets/Socket.IO

La diferencia más importante que veo es que con HTTP, el navegador siempre tiene que pedir algo para recibir una respuesta. Es como mandar un mensaje y esperar la respuesta. En cambio, con WebSockets (y con Socket.IO), se abre una conexión continua entre el navegador y el servidor. Eso permite que ambos puedan enviarse mensajes en cualquier momento, sin pedir permiso cada vez.

Este tipo de comunicación en tiempo real es ideal para cosas como chats, videojuegos, pizarras colaborativas o cualquier aplicación donde lo que pasa debe verse al instante en el otro lado.




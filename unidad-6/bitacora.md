# Evidencias de la unidad 6

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


# Envío de confirmaciones por correo

Para enviar los datos de los formularios por email se utiliza [FormSubmit](https://formsubmit.co), de modo que no es necesario programar un backend propio.

1. Registrá la cuenta `casoriolauymanu@gmail.com` en el sitio de FormSubmit.
2. El formulario puede apuntar directamente al servicio con:
   ```html
   <form action="https://formsubmit.co/el/digaza" method="POST">
   ```
3. En el código se usa la versión AJAX de la URL para mostrar un mensaje sin abandonar la página:
   ```javascript
   const emailServiceURL = 'https://formsubmit.co/ajax/el/digaza';
   fetch(emailServiceURL, { method: 'POST', body: new FormData(form) })
     .then(() => M.toast({html: '¡Confirmación enviada! Gracias.'}))
     .catch(() => M.toast({html: 'Ocurrió un error, intentá nuevamente.'}));
   ```
4. Con esto cada envío llegará a la casilla indicada con los datos del formulario.

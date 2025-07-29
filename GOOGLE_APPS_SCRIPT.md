# Envío de confirmaciones por correo

Para que cada respuesta del formulario se envíe por correo electrónico podés usar un Google Apps Script sencillo.

1. Abrí [Google Apps Script](https://script.google.com/) con tu cuenta de Gmail y creá un proyecto nuevo.
2. Pegá el siguiente código:

```javascript
function doPost(e) {
  var datos = e.parameters;
  var texto = Object.keys(datos).map(function(k) {
    var v = datos[k];
    if (Array.isArray(v)) v = v.join(', ');
    return k + ': ' + v;
  }).join('\n');
  MailApp.sendEmail('casoriolauymanu@gmail.com', 'Nueva confirmación', texto);
  return ContentService.createTextOutput('ok');
}
```

3. Desplegalo desde **Deploy -> New deployment** como **Web app** y permití que sea ejecutado por *Anyone*.
4. Copiá la URL que genera el despliegue y usala como `action` o en la llamada `fetch` de cada formulario.

Con esto cada envío llegará directamente a `casoriolauymanu@gmail.com` con los datos de confirmación.

# Registro de confirmaciones en Google Sheets

Para guardar cada respuesta del formulario en la hoja de cálculo y enviar una copia por correo se puede usar un Google Apps Script.

1. Abrir [la planilla](https://docs.google.com/spreadsheets/d/10rUuW9rKVIxR3e18DWR321lsH4GZBVlpv_r6dq8qZ_c/edit?usp=sharing) con la cuenta de Gmail.
2. Crear un nuevo script desde `Extensiones -> Apps Script` y pegar el siguiente código:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Respuestas');
  if (!sheet) {
    sheet = SpreadsheetApp.getActiveSpreadsheet().insertSheet('Respuestas');
  }
  sheet.appendRow([
    new Date(),
    e.parameter.nombre,
    e.parameter.asiste,
    e.parameter.restricciones
  ]);

  MailApp.sendEmail('casoriolauymanu@gmail.com', 'Nueva confirmación',
    'Nombre: ' + e.parameter.nombre + '\n' +
    'Asiste: ' + e.parameter.asiste + '\n' +
    'Restricciones: ' + (e.parameter.restricciones || 'Sin datos'));

  return ContentService.createTextOutput('ok');
}
```

3. Guardar y desplegar el script seleccionando **Deploy -> New deployment**, tipo **Web app**. Elegir que pueda ser ejecutado por *Anyone*.
4. Copiar la URL que genera el despliegue. En este proyecto ya se configuró la URL
   `https://script.google.com/macros/s/AKfycbw_Cf0X778WWDDKCoT0MUgEv3Tce90fjXv252auaELvaYiioGBuKIGzwcZUkbijyfUx/exec`
   para que cada formulario envíe sus datos automáticamente.

Con esa configuración, cada vez que se envíe el formulario los datos se agregarán a la hoja y se enviará un mail a la cuenta indicada.

**Nota:** No ejecutes la función *doPost* manualmente desde el editor porque el parámetro de evento `e` estará indefinido y aparecerá un error como 'Cannot read properties of undefined (reading \'parameter\')'. Debe desplegarse el script como Web App y luego enviarle la solicitud POST desde el formulario.
Si al enviar el formulario obtienes un "401 Unauthorized" revisa la configuración del despliegue. Debe estar habilitada la opción *Anyone* para permitir el acceso anónimo. Asegúrate también de usar la URL correcta que genera el despliegue.

Un error "405 Method Not Allowed" suele indicar que se abrió la URL del script directamente en el navegador. El Apps Script solo acepta solicitudes POST desde los formularios.

# plus_delete_with_help_button

`plus_delete_with_help_button` es una **función asincrónica que elimina un registro en el servidor mostrando primero una confirmación al usuario**.

La función forma parte de un **flujo estándar de eliminación segura** en interfaces de usuario.

En otras palabras:

**Confirmar → enviar solicitud al servidor → mostrar resultado.**

---

# Qué hace esta función

La función realiza el siguiente flujo:

```
usuario intenta eliminar
        ↓
mostrar confirmación
        ↓
usuario acepta
        ↓
enviar petición al servidor
        ↓
si éxito → mostrar mensaje de éxito
si error → mostrar alerta de error
```

Esto evita eliminaciones accidentales.

---

# Código fuente

```javascript
async function plus_delete_with_help_button(id, link, title = '', message = '' ,title_success='', message_success='') {
```

Es una **función asincrónica**, por lo que utiliza `await` para esperar respuestas.

---

# Parámetros

La función recibe varios parámetros.

---

## id

Identificador del registro que se desea eliminar.

```javascript
id
```

Ejemplo:

```
id = 25
```

---

## link

URL del endpoint del servidor que realizará la eliminación.

```javascript
link
```

Ejemplo:

```
/api/customer/delete
```

---

## title

Título del mensaje de confirmación.

```javascript
title
```

Si no se define, se usa una clave de traducción por defecto.

---

## message

Mensaje descriptivo que se mostrará al usuario.

```javascript
message
```

También admite traducción.

---

## title_success

Título del mensaje cuando la eliminación fue exitosa.

```javascript
title_success
```

---

## message_success

Mensaje mostrado cuando el registro fue eliminado correctamente.

```javascript
message_success
```

---

# Traducción de textos

La función permite que los mensajes se traduzcan automáticamente.

```javascript
const titleToTranslate = title || 'info.confirm_delete';
const messageToTranslate = message || 'info.description_delete';
```

Si el programador **no proporciona textos**, se usan claves de traducción.

Luego se traducen usando:

```javascript
const titleDelete = window.translate_text(titleToTranslate);
const messageDelete = window.translate_text(messageToTranslate);
```

Esto permite que el sistema funcione en **múltiples idiomas**.

---

# Mostrar confirmación

Después se muestra un cuadro de confirmación al usuario.

```javascript
if (await show_message_question(titleDelete, messageDelete)) {
```

Esta función probablemente muestra algo como:

```
Are you sure?

This action will permanently delete the record.

[ Cancel ] [ Delete ]
```

Si el usuario cancela, la función termina.

---

# Enviar solicitud al servidor

Si el usuario confirma, se envía una petición al backend.

```javascript
const answer = await send_message_to_the_server(link, { id }, true);
```

Se envía:

```
{ id: 25 }
```

al endpoint:

```
/api/delete
```

El servidor debe responder algo como:

```json
{
  "success": true
}
```

o

```json
{
  "success": false,
  "error": "Record not found"
}
```

---

# Si la eliminación fue exitosa

Si el servidor confirma que todo salió bien:

```javascript
if (answer.success) {
```

Se muestra un mensaje de éxito.

```javascript
const title = title_success || 'success.deleted';
const message = message_success || 'success.deleted';

show_alert('success', title, message);
```

Ejemplo visual:

```
Success
Record deleted successfully
```

Luego la función devuelve:

```
true
```

Esto permite que otros componentes sepan que la eliminación fue exitosa.

---

# Si ocurre un error

Si el servidor devuelve un error:

```javascript
show_alert(
  'alert',
  window.t('error.general'),
  window.t('error.to_delete'),
  answer.error
);
```

Se muestra un mensaje de alerta al usuario.

También se registra el error en la consola:

```javascript
console.error(answer.message);
console.error(answer.error);
```

Esto ayuda al desarrollador a depurar problemas.

---

# Valor de retorno

La función devuelve un valor booleano.

```
true  → eliminación exitosa
false → cancelado o error
```

Esto permite usar la función en flujos más grandes.

---

# Ejemplo de uso

```javascript
await plus_delete_with_help_button(
  15,
  '/api/customer/delete'
);
```

Flujo:

```
usuario presiona eliminar
        ↓
confirmación
        ↓
DELETE /api/customer/delete
        ↓
registro eliminado
```

---

# Ejemplo con mensajes personalizados

```javascript
await plus_delete_with_help_button(
  10,
  '/api/product/delete',
  'Delete product',
  'Do you really want to delete this product?',
  'Product deleted',
  'The product was successfully removed'
);
```

Esto mostrará mensajes personalizados.

---

# Ejemplo dentro de un botón

```javascript
deleteBtn.addEventListener('click', async () => {
  if (await plus_delete_with_help_button(
      product.id,
      '/api/product/delete'
  )) {
      reload_products();
  }
});
```

Si la eliminación fue exitosa, se actualiza la lista.

---

# Ventajas de esta función

Esta función centraliza la lógica de eliminación:

* evita código repetido
* muestra confirmaciones automáticamente
* maneja errores del servidor
* soporta traducciones
* devuelve resultado para control de flujo
* mejora la experiencia de usuario

---

# Flujo completo

```
usuario presiona eliminar
        ↓
plus_delete_with_help_button()
        ↓
mostrar confirmación
        ↓
usuario acepta
        ↓
enviar petición al servidor
        ↓
respuesta del servidor
        ↓
mostrar éxito o error
        ↓
retornar resultado
```

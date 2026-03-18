# show_message_question

`show_message_question` es una **función asíncrona** utilizada para mostrar un **popup de confirmación** al usuario y esperar su respuesta.

Esta función es un **wrapper de alto nivel** sobre `ConfirmDialog.show()` que agrega:

* Manejo automático de **z-index**
* Soporte para **traducciones dinámicas**
* Soporte para **variables dentro del texto**
* Compatibilidad con diferentes formatos de entrada

El resultado de la función es un **booleano** que indica si el usuario aceptó o canceló la acción.

---

## Firma de la función

```javascript id="c9m8xt"
async function show_message_question(title, message)
```

### Parámetros

| Parámetro | Tipo                        | Descripción       |
| --------- | --------------------------- | ----------------- |
| `title`   | `string \| array \| object` | Título del popup  |
| `message` | `string \| array \| object` | Mensaje del popup |

Los parámetros aceptan las mismas estructuras que `get_text_and_keys`.

---

## Retorno

```javascript id="a4d3p2"
Promise<boolean>
```

| Valor   | Significado                 |
| ------- | --------------------------- |
| `true`  | El usuario aceptó la acción |
| `false` | El usuario canceló          |

---

## Uso básico

```javascript id="rx8v7n"
const confirmed = await show_message_question(
  "Confirm action",
  "Are you sure?"
);

if (confirmed) {
  console.log("User accepted");
}
```

---

## Ejemplo con variables dinámicas

La función soporta textos con variables dinámicas.

```javascript id="y9rjv1"
await show_message_question(
  ["Hello ${name}", { name: "Edward" }],
  ["You have ${count} messages", { count: 3 }]
);
```

Resultado mostrado al usuario:

```text id="dyn-text-example"
Hello Edward
You have 3 messages
```

---

## Ejemplo con array compacto

También se puede usar el formato de array corto:

```javascript id="d37u1s"
await show_message_question(
  ["Hello ${}", ["Edward"]],
  ["You have ${} messages", [3]]
);
```

---

## Manejo de capas (z-index)

La función controla el orden visual de los popups usando la variable global:

```javascript id="3rpxj2"
currentPopZIndex
```

Cada vez que se llama la función:

```javascript id="8p6wq1"
currentPopZIndex += 1
```

Esto asegura que el popup aparezca **encima de otros elementos de la interfaz**.

---

## Integración con el menú de aplicaciones

La función también actualiza el `z-index` del menú principal del sistema:

```javascript id="p3qkfe"
const appMenu = document.getElementById('appMenu');
```

Si el menú ya tiene un `z-index`, se incrementa:

```javascript id="9t5c7m"
newZIndex = currentZIndex + 1
```

Si no tiene, se asigna uno por defecto:

```javascript id="zindex-default"
1000
```

Esto evita conflictos visuales entre el popup y el menú.

---

## Flujo de la función

```text id="flow-message-question"
show_message_question()
        ↓
Incrementa currentPopZIndex
        ↓
Actualiza z-index del menú
        ↓
Procesa title y message
        ↓
get_text_and_keys()
        ↓
translate_text()
        ↓
ConfirmDialog.show()
        ↓
Usuario responde
        ↓
Retorna true / false
```

---

## Ejemplo real

```javascript id="real-example-confirm"
async function deleteUser(name) {

  const confirmed = await show_message_question(
    ["Delete user ${name}", { name }],
    "Do you really want to delete this user?"
  );

  if (!confirmed) return;

  console.log("User deleted");
}
```

---

## Código fuente

```javascript id="source-show-message-question"
async function show_message_question(title, message) {

  currentPopZIndex += 1

  const appMenu = document.getElementById('appMenu');
  let currentZIndex = window.getComputedStyle(appMenu).getPropertyValue('z-index');

  if (!isNaN(currentZIndex)) {
    let newZIndex = currentZIndex + 1;
    appMenu.style.zIndex = newZIndex;
  } else {
    appMenu.style.zIndex = 1000;
  }

  let titleStruct = get_text_and_keys(title);
  let messageStruct = get_text_and_keys(message);

  const result = await ConfirmDialog.show(
    window.translate_text(titleStruct.text, titleStruct.keys),
    window.translate_text(messageStruct.text, messageStruct.keys)
  );

  return result;
}
```

---

## Ventajas

* Simplifica mostrar confirmaciones al usuario
* Permite usar **async/await**
* Soporta **variables dinámicas en textos**
* Maneja automáticamente **z-index**
* Compatible con el sistema de **traducciones del ERP**
* Centraliza la lógica de confirmaciones en una sola función

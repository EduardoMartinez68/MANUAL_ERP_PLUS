# set_value_plus_select

`set_value_plus_select` es una **función auxiliar (helper)** que permite **establecer el valor de un componente `plus-select` desde JavaScript usando su `id`**.

Su objetivo principal es **simplificar el acceso al método `setValue()` del componente** sin tener que buscar el elemento manualmente o comprobar si el método existe.

En resumen:

**`set_value_plus_select` = forma rápida y segura de cambiar el valor de un `plus-select`.**

---

# Código fuente

```javascript
function set_value_plus_select(id, newValue, newText = null) {
  const mySelect = document.getElementById(id);
  if (!mySelect) return;

  if (typeof mySelect.setValue === 'function') {
    mySelect.setValue(newValue, newText);
  }
}
```

---

# Qué hace esta función

La función realiza tres pasos principales:

```
1. Buscar el componente en el DOM
2. Verificar que el componente tenga el método setValue
3. Asignar el nuevo valor
```

Esto evita errores si el elemento no existe o no es un `plus-select`.

---

# Parámetros

La función recibe **tres parámetros**.

---

# id

Identificador del componente `plus-select`.

```javascript
id
```

Debe ser el mismo valor que se definió en el atributo `id` del componente.

Ejemplo:

```html
<plus-select id="customerSelect"></plus-select>
```

Uso:

```
customerSelect
```

---

# newValue

Es el **valor que se asignará al select**.

```javascript
newValue
```

Ejemplo:

```
5
```

Esto significa que el `plus-select` seleccionará la opción con valor `5`.

---

# newText (opcional)

Es el **texto que se mostrará en el select**.

```javascript
newText
```

Este parámetro es opcional.

Si no se proporciona, el componente intentará obtener el texto automáticamente.

Ejemplo:

```
"Edward"
```

---

# Paso 1 — Buscar el elemento

La función primero intenta encontrar el elemento en el DOM.

```javascript
const mySelect = document.getElementById(id);
```

Ejemplo:

```
id = "customerSelect"
```

Resultado:

```
<plus-select id="customerSelect">
```

---

# Paso 2 — Verificar que existe

Si el elemento no existe, la función termina inmediatamente.

```javascript
if (!mySelect) return;
```

Esto evita errores como:

```
Cannot read property 'setValue' of null
```

---

# Paso 3 — Verificar el método setValue

La función comprueba que el componente tenga el método `setValue`.

```javascript
if (typeof mySelect.setValue === 'function')
```

Esto asegura que el elemento sea compatible con la API del componente.

---

# Paso 4 — Establecer el valor

Si todo es correcto, se ejecuta el método:

```javascript
mySelect.setValue(newValue, newText);
```

Esto actualiza el componente visualmente y también su valor interno.

---

# Ejemplo básico

HTML:

```html
<plus-select
id="customer"
name="customer">
</plus-select>
```

JavaScript:

```javascript
set_value_plus_select("customer", 5);
```

Resultado:

```
Customer seleccionado → ID 5
```

---

# Ejemplo con texto

```javascript
set_value_plus_select("customer", 5, "Edward");
```

Resultado visual:

```
Edward
```

Esto es útil cuando el texto aún **no existe en el select**.

---

# Ejemplo con datos del servidor

```javascript
fetch("/api/customer/5")
  .then(r => r.json())
  .then(data => {
    set_value_plus_select(
      "customer",
      data.id,
      data.name
    );
  });
```

Esto establece el valor usando datos obtenidos desde una API.

---

# Ejemplo en formularios

```javascript
function loadInvoice(invoice) {
  set_value_plus_select(
    "customer",
    invoice.customer_id,
    invoice.customer_name
  );
}
```

Esto permite **cargar valores automáticamente en formularios de edición**.

---

# Ventajas de esta función

Usar esta función tiene varias ventajas:

* simplifica el código
* evita errores si el elemento no existe
* verifica que el método `setValue` esté disponible
* reduce código repetido
* facilita trabajar con `plus-select`

---

# Comparación con acceso directo

Sin helper:

```javascript
const select = document.getElementById("customer");

if (select && typeof select.setValue === "function") {
  select.setValue(5, "Edward");
}
```

Con helper:

```javascript
set_value_plus_select("customer", 5, "Edward");
```

Mucho más simple.

---

# Flujo de funcionamiento

```
set_value_plus_select()
        ↓
buscar elemento por id
        ↓
verificar que existe
        ↓
verificar método setValue
        ↓
actualizar valor del select
```

---

# Cuándo usar esta función

Es útil cuando:

* se cargan datos desde el servidor
* se rellenan formularios de edición
* se actualizan selects dinámicamente
* se quiere evitar errores de JavaScript

---

# Conclusión

`set_value_plus_select` es un **helper pequeño pero muy útil** para interactuar con el componente `plus-select` desde JavaScript.

Permite **establecer valores de forma segura, simple y reutilizable** en cualquier parte de la aplicación.

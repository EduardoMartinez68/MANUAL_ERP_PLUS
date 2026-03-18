# get_value_plus_select

`get_value_plus_select` es una **función auxiliar (helper)** que permite **obtener el valor seleccionado de un componente `plus-select` usando su `id`**.

Esta función sirve para **leer el valor actual del select desde JavaScript** de forma segura.

En resumen:

**`get_value_plus_select` = forma sencilla de obtener el valor seleccionado de un `plus-select`.**

---

# Código fuente

```javascript
function get_value_plus_select(id) {
  const mySelect = document.getElementById(id);
  if (!mySelect) return null;

  if (typeof mySelect.getValue === 'function') {
    return mySelect.getValue();
  }
  return null;
}
```

---

# Qué hace esta función

La función realiza tres pasos principales:

```
1. Buscar el componente en el DOM
2. Verificar que tenga el método getValue()
3. Devolver el valor seleccionado
```

Esto evita errores si el componente no existe o si el método no está disponible.

---

# Parámetros

La función recibe **un parámetro**.

---

# id

Es el `id` del componente `plus-select`.

```javascript
id
```

Debe coincidir con el atributo `id` definido en el HTML.

Ejemplo:

```html
<plus-select id="customer"></plus-select>
```

Uso:

```
customer
```

---

# Paso 1 — Buscar el componente

Primero se busca el elemento en el DOM.

```javascript
const mySelect = document.getElementById(id);
```

Ejemplo:

```
id = "customer"
```

Resultado:

```html
<plus-select id="customer">
```

---

# Paso 2 — Verificar que exista

Si el elemento no existe, la función devuelve `null`.

```javascript
if (!mySelect) return null;
```

Esto evita errores como:

```
Cannot read property 'getValue' of null
```

---

# Paso 3 — Verificar el método getValue

La función comprueba que el componente tenga el método `getValue`.

```javascript
if (typeof mySelect.getValue === 'function')
```

Esto asegura que el elemento sea compatible con la API del componente.

---

# Paso 4 — Obtener el valor

Si todo es correcto, se obtiene el valor con:

```javascript
return mySelect.getValue();
```

Este método devuelve el **valor seleccionado actualmente en el componente**.

---

# Valor de retorno

La función puede devolver dos tipos de resultado:

```
1. El valor seleccionado
2. null si ocurre algún problema
```

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
const customerId = get_value_plus_select("customer");
```

Resultado:

```
customerId = 5
```

---

# Ejemplo en un formulario

```javascript
function saveInvoice() {

  const customerId = get_value_plus_select("customer");

  console.log(customerId);

}
```

Resultado:

```
5
```

Esto permite enviar el valor al servidor.

---

# Ejemplo con validación

```javascript
const customerId = get_value_plus_select("customer");

if (!customerId) {
  alert("Seleccione un cliente");
}
```

Esto sirve para validar formularios.

---

# Ejemplo con envío a una API

```javascript
const data = {
  customer_id: get_value_plus_select("customer")
};

fetch("/api/invoice", {
  method: "POST",
  body: JSON.stringify(data)
});
```

Resultado enviado al servidor:

```json
{
  "customer_id": 5
}
```

---

# Comparación con acceso directo

Sin helper:

```javascript
const select = document.getElementById("customer");

let value = null;

if (select && typeof select.getValue === "function") {
  value = select.getValue();
}
```

Con helper:

```javascript
const value = get_value_plus_select("customer");
```

Mucho más limpio y reutilizable.

---

# Flujo de funcionamiento

```
get_value_plus_select()
        ↓
buscar elemento por id
        ↓
verificar que existe
        ↓
verificar método getValue
        ↓
devolver valor seleccionado
```

---

# Cuándo usar esta función

Es útil cuando:

* se envían formularios
* se necesitan valores seleccionados
* se realizan validaciones
* se envían datos a una API
* se quiere evitar errores de JavaScript

---

# Ventajas

Usar esta función tiene varias ventajas:

* simplifica el código
* evita errores si el componente no existe
* centraliza la lógica de acceso
* mejora la legibilidad del código
* es reutilizable en toda la aplicación

---

# Conclusión

`get_value_plus_select` es un **helper simple pero muy útil** para interactuar con el componente `plus-select`.

Permite **obtener el valor seleccionado de forma segura y reutilizable**, facilitando el manejo de formularios y la comunicación con el servidor.

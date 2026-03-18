# get_status_plus_switch

`get_status_plus_switch` es una **función auxiliar (helper)** que permite **obtener el estado actual de un componente `plus-switch`** usando su `id`.

Esta función sirve para saber si el switch está:

* **activado (`true`)**
* **desactivado (`false`)**

En resumen:

**`get_status_plus_switch` = obtiene el estado ON/OFF de un `plus-switch`.**

---

# Código fuente

```javascript
function get_status_plus_switch(id) {
  const mySwitch = document.getElementById(id);
  if (!mySwitch) return;

  // her we will see if the component has a shadowRoot
  const shadow = mySwitch.shadowRoot;
  if (!shadow) return;

  const input = shadow.getElementById(id);
  return input.checked ? input.checked : false;
}
```

---

# Qué hace esta función

La función sigue estos pasos:

```
1. Buscar el componente en el DOM
2. Acceder a su Shadow DOM
3. Obtener el input checkbox interno
4. Leer su propiedad checked
5. Devolver true o false
```

Esto permite **consultar el estado interno del switch**.

---

# Parámetros

La función recibe **un parámetro**.

---

# id

Es el `id` del componente `plus-switch`.

```javascript
id
```

Debe coincidir con el atributo `id` del componente.

Ejemplo:

```html
<plus-switch id="notifications"></plus-switch>
```

Uso:

```
notifications
```

---

# Paso 1 — Buscar el componente

Primero se busca el elemento en el DOM.

```javascript
const mySwitch = document.getElementById(id);
```

Ejemplo:

```
id = "notifications"
```

Resultado:

```html
<plus-switch id="notifications">
```

Si el elemento no existe la función termina.

```javascript
if (!mySwitch) return;
```

Esto evita errores como:

```
Cannot read properties of null
```

---

# Paso 2 — Acceder al Shadow DOM

Los **Web Components** pueden encapsular su HTML interno dentro de un **Shadow DOM**.

Por eso la función intenta acceder a él.

```javascript
const shadow = mySwitch.shadowRoot;
```

Si el componente no tiene `shadowRoot`, la función termina.

```javascript
if (!shadow) return;
```

---

# Qué es el Shadow DOM

El **Shadow DOM** es una característica de los Web Components que permite **encapsular HTML y CSS dentro de un componente**.

Esto evita conflictos con el resto de la página.

Ejemplo de estructura:

```
<plus-switch>
   #shadow-root
      <input type="checkbox">
      <span class="slider">
</plus-switch>
```

El contenido dentro del `#shadow-root` no es accesible directamente desde el DOM principal.

---

# Paso 3 — Obtener el input interno

Dentro del Shadow DOM se busca el `checkbox`.

```javascript
const input = shadow.getElementById(id);
```

Esto obtiene el elemento:

```html
<input type="checkbox">
```

Este checkbox es el que controla el estado del switch.

---

# Paso 4 — Obtener el estado

El estado del switch se obtiene con:

```javascript
input.checked
```

Esto devuelve:

```
true
false
```

---

# Paso 5 — Retornar el resultado

La función devuelve:

```javascript
return input.checked ? input.checked : false;
```

Esto garantiza que el resultado siempre sea:

```
true
false
```

---

# Valor de retorno

La función puede devolver:

```
true  → switch activado
false → switch apagado
undefined → si el componente no existe
```

---

# Ejemplo básico

HTML:

```html
<plus-switch
id="notifications"
text="Notifications">
</plus-switch>
```

JavaScript:

```javascript
const status = get_status_plus_switch("notifications");
console.log(status);
```

Resultado posible:

```
true
```

---

# Ejemplo en formulario

```javascript
function saveSettings(){

  const notifications =
  get_status_plus_switch("notifications");

  console.log(notifications);

}
```

Resultado:

```
true
```

Esto permite enviar el estado al servidor.

---

# Ejemplo con API

```javascript
const data = {
  notifications:
  get_status_plus_switch("notifications")
};

fetch("/api/settings", {
  method: "POST",
  body: JSON.stringify(data)
});
```

Resultado enviado al servidor:

```json
{
 "notifications": true
}
```

---

# Comparación con acceso manual

Sin helper:

```javascript
const sw = document.getElementById("notifications");
const status = sw.shadowRoot
  .getElementById("notifications")
  .checked;
```

Con helper:

```javascript
const status =
get_status_plus_switch("notifications");
```

Mucho más simple.

---

# Flujo de funcionamiento

```
get_status_plus_switch()
        ↓
buscar componente
        ↓
acceder al shadowRoot
        ↓
buscar checkbox interno
        ↓
leer propiedad checked
        ↓
retornar true o false
```

---

# Cuándo usar esta función

Es útil cuando:

* se envían formularios
* se guardan configuraciones
* se necesita validar el estado de un switch
* se envían datos a una API
* se trabaja con Web Components que usan Shadow DOM

---

# Ventajas

Usar esta función tiene varias ventajas:

* simplifica el acceso al estado del switch
* evita errores si el elemento no existe
* encapsula la lógica del Shadow DOM
* mejora la legibilidad del código
* es reutilizable

---

# Nota importante

En muchos casos también se podría usar el método del componente:

```javascript
document
  .getElementById("notifications")
  .getChecked();
```

Pero `get_status_plus_switch` permite **acceder directamente al estado interno del switch**.

---

# Conclusión

`get_status_plus_switch` es un **helper útil para obtener el estado de un `plus-switch`**.

Permite acceder al valor `true/false` del switch incluso cuando el componente usa **Shadow DOM**, simplificando el código y evitando errores al trabajar con Web Components.

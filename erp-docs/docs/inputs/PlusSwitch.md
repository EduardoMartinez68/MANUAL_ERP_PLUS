# PlusSwitch

`PlusSwitch` es un **Web Component personalizado** que crea un **switch (interruptor) interactivo** para activar o desactivar una opción en una interfaz web.

Este componente encapsula toda la lógica necesaria para:

* mostrar un interruptor visual
* manejar su estado (`on / off`)
* integrarlo en formularios
* ejecutar funciones cuando cambia
* mostrar texto traducido
* mostrar iconos
* mostrar mensajes informativos

En resumen:

**`PlusSwitch` = un componente HTML reutilizable para crear interruptores tipo toggle.**

---

# Código base

```javascript
class PlusSwitch extends HTMLElement
```

Esto significa que el componente **extiende un elemento HTML nativo** usando la API de **Web Components**.

Después de registrarlo con:

```javascript
customElements.define("plus-switch", PlusSwitch);
```

se puede usar directamente en HTML:

```html
<plus-switch text="Active"></plus-switch>
```

---

# Estructura general del componente

El componente tiene tres partes principales:

```
constructor()
connectedCallback()
métodos públicos
```

Cada parte cumple una función específica.

---

# constructor()

```javascript
constructor() {
  super();
  this._checkbox = null;
  this._rendered = false;
}
```

El constructor inicializa el estado del componente.

## Variables internas

### `_checkbox`

Guarda una referencia al input tipo checkbox.

```
this._checkbox
```

Esto permite manipular el estado del switch desde otros métodos.

---

### `_rendered`

```
this._rendered = false
```

Este **flag evita que el componente se renderice múltiples veces**.

Esto es importante porque `connectedCallback()` puede ejecutarse varias veces en ciertos casos del DOM.

---

# connectedCallback()

```javascript
connectedCallback()
```

Este método se ejecuta **automáticamente cuando el componente se agrega al DOM**.

Aquí es donde el componente **crea toda su estructura HTML interna**.

---

# Evitar render duplicado

Lo primero que hace el método es comprobar si ya fue renderizado.

```javascript
if (this._rendered) return;
```

Si ya fue creado, el método termina.

Esto evita:

```
duplicación de elementos
eventos duplicados
problemas de rendimiento
```

---

# Obtener atributos del componente

El componente lee varios atributos definidos por el programador.

---

# text

```javascript
const text = this.getAttribute('text') || '';
```

Define el texto que se mostrará junto al switch.

Ejemplo:

```html
<plus-switch text="Enable notifications"></plus-switch>
```

---

# t (traducción)

```javascript
const t = this.getAttribute('t') || text;
```

Este atributo permite usar **claves de traducción**.

Luego se traduce usando:

```javascript
window.translate_text(t)
```

Esto permite dos formas de uso:

```
text="Save"
```

o

```
t="save_button"
```

---

# name

```javascript
const name = this.getAttribute('name') || '';
```

Esto permite integrar el switch dentro de formularios.

Ejemplo:

```html
<plus-switch name="active"></plus-switch>
```

---

# checked / value

Determina si el switch inicia activado.

```javascript
const valueCheck = this.getAttribute('checked') || this.getAttribute('value') || false;
```

Después se convierte a booleano usando:

```javascript
is_true(valueCheck)
```

Esto permite usar:

```html
<plus-switch checked="true"></plus-switch>
```

o

```html
<plus-switch value="1"></plus-switch>
```

---

# Crear contenedor principal

```javascript
const container = document.createElement('div');
```

Este contenedor guardará todo el componente.

---

# Wrapper del switch

```javascript
const wrapper = document.createElement('div');
wrapper.classList.add('plus-switch');
```

Esta clase controla el diseño visual mediante CSS.

---

# Crear texto del switch

```javascript
let labelText = document.createElement('span');
```

Se crea un `span` para mostrar el texto.

```javascript
labelText.classList.add('plus-switch-label');
```

También se guarda el atributo de traducción:

```javascript
labelText.setAttribute('t', t);
```

---

# Soporte para iconos

El componente permite agregar un icono opcional.

```javascript
const iconClass = this.getAttribute('icon');
```

Ejemplo:

```html
<plus-switch icon="fas fa-bell"></plus-switch>
```

Si existe un icono:

```javascript
const iconEl = document.createElement('i');
iconEl.className = iconClass;
```

El icono se inserta antes del texto.

---

# Agregar texto

Después del icono se agrega el texto:

```javascript
labelText.appendChild(
  document.createTextNode(textTranlsate)
);
```

---

# Mostrar mensaje informativo

El componente puede mostrar un mensaje informativo.

```javascript
const message = this.getAttribute('message');
```

Ejemplo:

```html
<plus-switch
text="Auto Save"
message="The system will save automatically">
</plus-switch>
```

Si existe el mensaje se crea:

```javascript
<info-label>
```

Este elemento muestra información adicional al usuario.

---

# Crear el switch visual

El interruptor se crea con:

```javascript
<label class="plus-switch-toggle">
```

Dentro se colocan dos elementos:

```
checkbox
slider visual
```

---

# Crear checkbox

```javascript
const checkbox = document.createElement('input');
checkbox.type = 'checkbox';
```

Se establece su estado inicial:

```javascript
checkbox.checked = isChecked;
```

También se asigna el `name` para formularios.

---

# Guardar referencia

```javascript
this._checkbox = checkbox;
```

Esto permite acceder al estado del switch desde otros métodos.

---

# Crear slider visual

```javascript
const slider = document.createElement('span');
slider.classList.add('plus-switch-slider');
```

Este elemento se estiliza con CSS para crear la animación del switch.

---

# Ensamblar el componente

Los elementos se agregan en este orden:

```
wrapper
 ├ labelText
 └ switchContainer
      ├ checkbox
      └ slider
```

Luego se agregan al contenedor principal.

---

# Insertar en el DOM

El contenido final se agrega al componente.

```javascript
this.appendChild(container);
```

---

# Manejar evento change

El componente escucha cuando el switch cambia.

```javascript
checkbox.addEventListener('change', (e) => {
```

Se obtiene el estado actual:

```javascript
const status = this._checkbox.checked;
```

---

# Ejecutar función onclick

El programador puede definir una función que se ejecutará al cambiar el switch.

Ejemplo:

```html
<plus-switch onclick="switchChanged"></plus-switch>
```

Entonces el componente ejecuta:

```javascript
window[eventOnclick](status);
```

Esto enviará:

```
true
false
```

según el estado del switch.

---

# Marcar como renderizado

Finalmente:

```javascript
this._rendered = true;
```

Esto evita que el componente se renderice nuevamente.

---

# Métodos públicos

El componente expone dos métodos para interactuar desde JavaScript.

---

# setChecked()

Permite cambiar el estado del switch.

```javascript
setChecked(value)
```

Ejemplo:

```javascript
document
  .querySelector("plus-switch")
  .setChecked(true);
```

Resultado:

```
Switch activado
```

---

# getChecked()

Devuelve el estado actual del switch.

```javascript
getChecked()
```

Ejemplo:

```javascript
const status =
document
  .querySelector("plus-switch")
  .getChecked();
```

Resultado:

```
true
false
```

---

# Ejemplo completo

HTML:

```html
<plus-switch
id="notifications"
text="Notifications"
icon="fas fa-bell"
name="notifications"
checked="true"
onclick="onSwitchChange">
</plus-switch>
```

JavaScript:

```javascript
function onSwitchChange(status){
  console.log("Switch status:", status);
}
```

---

# Flujo de funcionamiento

```
plus-switch se agrega al DOM
        ↓
connectedCallback()
        ↓
leer atributos
        ↓
crear estructura HTML
        ↓
insertar checkbox y slider
        ↓
agregar evento change
        ↓
ejecutar función onclick
```

---

# Ventajas del componente

Usar `PlusSwitch` tiene varias ventajas:

* componente reutilizable
* encapsula lógica y diseño
* fácil integración en formularios
* soporta traducciones
* soporta iconos
* soporta mensajes informativos
* permite controlar el estado desde JavaScript

---

# Conclusión

`PlusSwitch` es un **componente personalizado basado en Web Components** que permite crear interruptores interactivos modernos en una aplicación web.

Encapsula **lógica, diseño y comportamiento en un solo elemento HTML**, facilitando el desarrollo de interfaces reutilizables y mantenibles.

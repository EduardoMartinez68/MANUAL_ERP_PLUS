# PlusActions

`PlusActions` es un **Web Component** que permite crear **menús de acciones dinámicos** similares a los menús de opciones que aparecen en tablas, dashboards o aplicaciones profesionales.

Este componente genera:

* un **botón de acciones**
* un **menú desplegable**
* **acciones con iconos**
* **switches interactivos**
* **atajos rápidos (quick actions)**
* **soporte para atajos de teclado**
* **sistema de "pin" para fijar acciones**

---

# Propósito del componente

`PlusActions` sirve para mostrar múltiples acciones en un espacio pequeño.

Ejemplo visual:

```
⋮
---------
Editar
Eliminar
Duplicar
Activar
---------
```

Además permite **fijar acciones como accesos rápidos**.

---

# Definición del componente

```javascript
class PlusActions extends HTMLElement
```

Esto define un **custom element** que puede utilizarse así:

```html
<plus-actions>
  <plus-action text="Edit" icon="fi fi-rr-edit" onclick="editRecord"></plus-action>
  <plus-action text="Delete" icon="fi fi-rr-trash" onclick="deleteRecord"></plus-action>
</plus-actions>
```

---

# constructor()

```javascript
constructor() {
  super();
  this.open = false;
}
```

### Propósito

Inicializa el estado del menú.

| propiedad | función                        |
| --------- | ------------------------------ |
| open      | indica si el menú está abierto |

---

# connectedCallback()

```javascript
connectedCallback()
```

Este método se ejecuta cuando el componente se agrega al DOM.

Aquí se realiza:

* creación del botón
* creación del menú
* lectura de acciones
* creación de accesos rápidos
* asignación de eventos

---

# Protección contra render duplicado

```javascript
if (this._rendered) return;
this._rendered = true;
```

Evita que el componente **se renderice varias veces**.

---

# Clase principal del componente

```javascript
this.classList.add('plus-actions');
```

Agrega la clase CSS:

```
.plus-actions
```

---

# Botón de acciones

```javascript
const button = document.createElement('button');
```

Este botón abre el menú.

Icono utilizado:

```html
<i class="fi fi-rr-menu-dots-vertical"></i>
```

Visualmente representa el clásico menú:

```
⋮
```

---

# Menú de acciones

```javascript
const menu = document.createElement('div');
menu.className = 'plus-action-menu';
```

Este contenedor almacenará todas las acciones.

Inicialmente está oculto:

```javascript
menu.style.display = 'none';
```

---

# Obtener acciones

```javascript
const actions = this.querySelectorAll('plus-action');
```

Esto obtiene todos los elementos:

```
<plus-action>
```

definidos dentro del componente.

---

# Atributos de las acciones

Cada acción puede tener varios atributos.

| atributo | función                   |
| -------- | ------------------------- |
| type     | tipo de acción            |
| icon     | icono                     |
| text     | texto visible             |
| t        | clave de traducción       |
| onclick  | función a ejecutar        |
| combo    | atajo de teclado          |
| pin      | permite fijar la acción   |
| checked  | estado inicial del switch |

---

# Soporte para atajos de teclado

Si una acción tiene atributo `combo`:

```html
<plus-action combo="ctrl+s">
```

Se crea un componente:

```javascript
<key-bind>
```

Código:

```javascript
const keyBind = document.createElement('key-bind');
keyBind.setAttribute('combo', combo)
keyBind.setAttribute('action', onclick)
```

Esto permite ejecutar la acción usando **atajos de teclado**.

---

# Traducción del texto

```javascript
const textTranslate = window.translate_text(t)
```

Esto permite que los textos se **traduzcan automáticamente**.

Si existe un atajo de teclado se agrega al texto:

```
Save Ctrl+S
```

---

# Tipos de acciones

El componente soporta **dos tipos principales**.

| tipo   | descripción     |
| ------ | --------------- |
| normal | acción estándar |
| switch | interruptor     |

---

# Acción tipo switch

Si el atributo `type="switch"`:

```html
<plus-action type="switch">
```

Se crea el componente:

```javascript
<plus-switch>
```

Código:

```javascript
item = document.createElement('plus-switch');
```

Este switch permite activar o desactivar opciones.

Ejemplo visual:

```
Notificaciones  [ON]
```

---

# Generación automática de ID

Si el programador no define un ID:

```javascript
const id = `plus-id-${Date.now()}-${Math.random()}`
```

Esto permite **identificar el switch posteriormente**.

---

# Acciones normales

Si no es switch, se crea un elemento:

```javascript
<div class="plus-action-item">
```

Dentro se genera la estructura visual.

---

# Estructura de acción

```html
<div class="item-row">
  <div class="item-main">
    <i class="icon"></i>
    <span>Texto</span>
  </div>
</div>
```

---

# Sistema de pin

Una acción puede tener atributo:

```html
pin="true"
```

Esto agrega un **icono de pin**.

Iconos posibles:

| icono           | significado |
| --------------- | ----------- |
| fi-ss-thumbtack | fijado      |
| fi-rs-thumbtack | no fijado   |

---

# Activar una acción

Si existe atributo `onclick`:

```javascript
item.onclick = () => {
  window[onclick]?.();
  this.toggle(false);
};
```

Esto ejecuta la función global.

Ejemplo:

```html
<plus-action onclick="deleteUser">
```

Ejecutará:

```
deleteUser()
```

---

# Evento del pin

El icono de pin tiene su propio evento.

```javascript
pinIcon.addEventListener('click')
```

Esto evita que el clic active la acción principal.

```javascript
e.stopPropagation();
```

---

# Quick Actions

El componente crea un contenedor:

```javascript
const quickActions = document.createElement('div');
quickActions.className = 'plus-quick-actions';
```

Este contenedor muestra **acciones fijadas como accesos rápidos**.

Ejemplo visual:

```
⚡  ✏️  🗑️
```

---

# toggle()

```javascript
toggle(forceState = null)
```

Este método **abre o cierra el menú**.

---

# Cambiar estado

```javascript
this.open = forceState !== null ? forceState : !this.open;
```

Esto permite:

* abrir
* cerrar
* alternar estado

---

# Mostrar menú

Si `open = true`:

```javascript
menu.style.display = 'block';
```

---

# Posicionamiento inteligente

El menú detecta el espacio disponible en pantalla.

Variables utilizadas:

```
triggerRect
menuRect
viewportWidth
```

Esto permite decidir si el menú debe abrir:

| dirección | condición   |
| --------- | ----------- |
| derecha   | hay espacio |
| izquierda | hay espacio |
| centrado  | fallback    |

---

# Animación del icono

Cuando el menú se abre:

```javascript
icon.style.transform = 'rotate(90deg)';
```

Cuando se cierra:

```javascript
icon.style.transform = 'rotate(0deg)';
```

---

# handlePin()

```javascript
handlePin(pinIcon, action, onclick, icon, textTranslate)
```

Este método controla el **sistema de fijado de acciones**.

---

# Desfijar acción

Si la acción ya está fijada:

```javascript
fi-ss-thumbtack → fi-rs-thumbtack
```

Se elimina el acceso rápido.

---

# Fijar acción

Si la acción no está fijada:

1. cambia el icono
2. se crea un botón rápido

```javascript
const shortcut = document.createElement('button');
```

---

# Acceso rápido

El acceso rápido puede mostrar:

* icono
* texto

```javascript
shortcut.innerHTML = `<i class="${icon}"></i>`
```

---

# Ejecutar acceso rápido

Al hacer clic:

```javascript
window[onclick]?.();
```

Esto ejecuta la misma acción del menú.

---

# Ejemplo completo

```html
<plus-actions>

  <plus-action
    text="Edit"
    icon="fi fi-rr-edit"
    onclick="editUser"
    pin="true">
  </plus-action>

  <plus-action
    text="Delete"
    icon="fi fi-rr-trash"
    onclick="deleteUser">
  </plus-action>

  <plus-action
    type="switch"
    text="Active"
    icon="fi fi-rr-check">
  </plus-action>

</plus-actions>
```

---

# Flujo de funcionamiento

```
plus-actions
      ↓
leer plus-action
      ↓
crear menú dinámico
      ↓
mostrar acciones
      ↓
clic en botón
      ↓
ejecutar función
```

---

# Ventajas del componente

`PlusActions` ofrece varias funcionalidades avanzadas:

* menú de acciones compacto
* soporte de iconos
* switches integrados
* accesos rápidos
* atajos de teclado
* sistema de pin
* posicionamiento inteligente
* integración con funciones JavaScript

---

# Conclusión

`PlusActions` es un **componente avanzado para manejar acciones dentro de interfaces web**, ideal para dashboards, tablas de datos o aplicaciones complejas donde se requiere mostrar múltiples acciones de forma ordenada y eficiente.

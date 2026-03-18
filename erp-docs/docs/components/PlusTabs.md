# PlusTabs

`PlusTabs` es un **Web Component** que permite crear **interfaces con pestañas (tabs)** dentro de una página web.

Este componente funciona junto con `PlusTab` y permite dividir contenido en **secciones navegables mediante botones**.

Ejemplo visual:

```
[ General ] [ Productos ] [ Clientes ]

Contenido del tab seleccionado...
```

---

# Componentes involucrados

El sistema está formado por **dos componentes**:

| Componente | Función                              |
| ---------- | ------------------------------------ |
| PlusTabs   | contenedor principal de las pestañas |
| PlusTab    | contenido de cada pestaña            |

---

# Estructura básica de uso

Ejemplo en HTML:

```html
<plus-tabs>

  <plus-tab text="General">
    Contenido de la pestaña General
  </plus-tab>

  <plus-tab text="Productos">
    Contenido de la pestaña Productos
  </plus-tab>

  <plus-tab text="Clientes">
    Contenido de la pestaña Clientes
  </plus-tab>

</plus-tabs>
```

Resultado:

* se generan automáticamente los **botones de navegación**
* cada botón mostrará su contenido correspondiente

---

# Clase PlusTabs

```javascript
class PlusTabs extends HTMLElement
```

Define el **contenedor principal de las pestañas**.

Este componente:

* genera los botones de navegación
* controla qué tab se muestra
* oculta los demás contenidos

---

# connectedCallback()

```javascript
connectedCallback()
```

Este método se ejecuta cuando el componente se **agrega al DOM**.

Aquí se realiza toda la lógica de:

* creación de botones
* lectura de tabs
* asignación de eventos
* mostrar/ocultar contenido

---

# Protección contra render duplicado

```javascript
if (this._rendered) return;
this._rendered = true;
```

Esto evita que el componente **se renderice más de una vez**.

Si `connectedCallback` se ejecuta nuevamente, el código se detiene.

---

# Clase CSS del contenedor

```javascript
this.classList.add('plus-tabs');
```

Agrega la clase:

```
.plus-tabs
```

Esto permite aplicar estilos al contenedor principal.

---

# Contenedor de botones

```javascript
const tabButtonsContainer = document.createElement('div');
tabButtonsContainer.className = 'plus-tab-buttons';
```

Se crea dinámicamente el contenedor donde estarán los botones.

Resultado HTML:

```html
<div class="plus-tab-buttons"></div>
```

---

# Obtener los tabs

```javascript
const tabs = Array.from(this.querySelectorAll('plus-tab'));
```

Esto obtiene todos los elementos:

```
<plus-tab>
```

dentro del componente.

Cada uno representa **una pestaña de contenido**.

---

# Crear botones de navegación

Por cada `plus-tab` se crea un botón.

```javascript
tabs.forEach((tab, index) => {
```

---

# Obtener el título del tab

```javascript
const tAttr = tab.getAttribute('t') || tab.getAttribute('text') || 'tab';
```

Se obtiene el texto del tab usando este orden:

1. atributo `t`
2. atributo `text`
3. valor por defecto `"tab"`

Ejemplo:

```html
<plus-tab text="Products"></plus-tab>
```

---

# Traducción del título

```javascript
const title = window.translate_text(tAttr);
```

Esto permite que los títulos **se traduzcan automáticamente** usando el sistema global de traducción.

---

# Crear botón

```javascript
const button = document.createElement('button');
```

Se genera el botón de navegación.

Propiedades asignadas:

```javascript
button.textContent = title;
button.type = 'button';
button.setAttribute('t', tAttr);
```

---

# Activar el primer tab

```javascript
if (index === 0) button.classList.add('active');
```

El primer botón se marca como activo.

Esto significa que **la primera pestaña será visible al iniciar**.

---

# Evento de clic

Cada botón tiene un evento:

```javascript
button.addEventListener('click', () => {
```

Este evento controla el cambio de pestañas.

---

# Mostrar el contenido correspondiente

```javascript
this.querySelectorAll('plus-tab').forEach((t, i) => {
  t.style.display = i === index ? 'block' : 'none';
});
```

La lógica es simple:

| condición        | acción     |
| ---------------- | ---------- |
| tab seleccionado | se muestra |
| otros tabs       | se ocultan |

---

# Actualizar estado de botones

Primero se eliminan los estados activos.

```javascript
tabButtonsContainer
  .querySelectorAll('button')
  .forEach(b => b.classList.remove('active'));
```

Luego se activa el botón seleccionado.

```javascript
button.classList.add('active');
```

---

# Agregar botones al contenedor

```javascript
tabButtonsContainer.appendChild(button);
```

Cada botón se añade al contenedor.

---

# Insertar botones en el DOM

```javascript
this.insertBefore(tabButtonsContainer, this.firstChild);
```

Esto coloca los botones **antes del contenido de los tabs**.

Resultado final:

```html
<plus-tabs>

  <div class="plus-tab-buttons">
    <button>Tab 1</button>
    <button>Tab 2</button>
  </div>

  <plus-tab>...</plus-tab>
  <plus-tab>...</plus-tab>

</plus-tabs>
```

---

# Mostrar solo el primer tab

Cuando la página carga se ejecuta:

```javascript
tabs.forEach((tab, index) => {
  tab.style.display = index === 0 ? 'block' : 'none';
});
```

Resultado:

| tab   | estado  |
| ----- | ------- |
| tab 1 | visible |
| tab 2 | oculto  |
| tab 3 | oculto  |

---

# Clase PlusTab

```javascript
class PlusTab extends HTMLElement
```

Representa **el contenido individual de cada pestaña**.

---

# connectedCallback()

```javascript
connectedCallback()
```

Cuando el elemento se agrega al DOM:

```javascript
this.classList.add('plus-tab');
```

Se agrega la clase CSS:

```
.plus-tab
```

Esto permite aplicar estilos al contenido.

---

# Ejemplo completo

```html
<plus-tabs>

  <plus-tab text="General">
    <h2>Configuración general</h2>
    <p>Opciones del sistema.</p>
  </plus-tab>

  <plus-tab text="Productos">
    <h2>Lista de productos</h2>
  </plus-tab>

  <plus-tab text="Clientes">
    <h2>Lista de clientes</h2>
  </plus-tab>

</plus-tabs>
```

---

# Flujo de funcionamiento

```
plus-tabs
     ↓
buscar plus-tab
     ↓
crear botones automáticamente
     ↓
mostrar primer tab
     ↓
click en botón
     ↓
mostrar tab correspondiente
```

---

# Ventajas del componente

`PlusTabs` ofrece varias ventajas:

* creación automática de navegación
* separación clara de contenido
* soporte de traducción
* reutilizable
* código HTML limpio
* interfaz más organizada

---

# Conclusión

`PlusTabs` es un **componente de interfaz para crear sistemas de pestañas dinámicos**, donde los botones se generan automáticamente a partir de los elementos `plus-tab`, permitiendo organizar grandes cantidades de contenido de forma clara y navegable.

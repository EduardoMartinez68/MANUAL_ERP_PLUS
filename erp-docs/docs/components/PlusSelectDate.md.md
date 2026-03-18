# PlusSelectDate

`PlusSelectDate` es un **Web Component personalizado** que permite seleccionar **rangos de fechas** de forma visual e interactiva.

El componente incluye:

* selección rápida de rangos
* selección personalizada de fechas
* soporte de traducción
* integración con formularios
* encapsulación mediante **Shadow DOM**

---

# Definición del componente

```javascript
class PlusSelectDate extends HTMLElement
```

Esto define un **custom element** que puede utilizarse directamente en HTML.

Ejemplo:

```html
<plus-select-date></plus-select-date>
```

---

# constructor()

```javascript
constructor() {
  super();
  this.attachShadow({ mode: 'open' });
}
```

### Qué hace

1. Llama al constructor de `HTMLElement`.
2. Crea un **Shadow DOM** para encapsular el componente.

---

# Shadow DOM

```javascript
this.attachShadow({ mode: 'open' });
```

Esto permite que:

* los estilos no afecten al resto de la página
* el HTML interno esté aislado
* el componente sea reutilizable

---

# connectedCallback()

```javascript
connectedCallback()
```

Este método se ejecuta automáticamente **cuando el componente se agrega al DOM**.

Aquí es donde se:

* leen atributos
* generan estilos
* crean los eventos
* construye la interfaz

---

# Lectura de atributos

## name-start

```javascript
const nameStart = this.getAttribute('name-start') || 'fecha_inicio';
```

Define el **nombre del input oculto** para la fecha inicial.

Ejemplo HTML:

```html
<plus-select-date name-start="date_from"></plus-select-date>
```

---

## name-end

```javascript
const nameEnd = this.getAttribute('name-end') || 'fecha_fin';
```

Define el **nombre del input oculto** para la fecha final.

---

## label

```javascript
const labelInfo = this.getAttribute('label') || window.t('btn.range_date');
```

Define el **texto del label** del componente.

Si no existe se usa la traducción:

```
btn.range_date
```

---

# Sistema de traducción

```javascript
const t = this.getAttribute('t') || labelInfo;
const labelText = window.translate_text(t)
```

El componente utiliza funciones globales:

```javascript
window.t()
window.translate_text()
```

Esto permite **mostrar textos en diferentes idiomas**.

---

# Atributo required

```javascript
const isRequired = this.hasAttribute('required');
```

Si el atributo existe:

```html
<plus-select-date required></plus-select-date>
```

Se agrega un **asterisco al label**:

```
Fecha *
```

---

# Idioma del navegador

```javascript
const lang = navigator.language || 'es-ES';
```

Esto se usa para **formatear las fechas según el idioma del usuario**.

---

# Traducciones utilizadas

El componente obtiene varios textos traducidos.

```javascript
const select_range_date
const today
const last_7_days
const current_month
const previous_month
const custom
const success
const cancel
```

Estos textos se usan para mostrar:

* Hoy
* Últimos 7 días
* Mes actual
* Mes anterior
* Personalizado
* Aceptar
* Cancelar

---

# Estructura HTML del componente

Dentro del `shadow.innerHTML` se crea toda la interfaz.

### Elementos principales

```
plus-select-date-wrapper
```

Contenedor principal del componente.

---

### Label

```html
<label class="plus-select-date-label">
```

Muestra el nombre del campo.

---

### Display del rango

```html
<div class="plus-select-date-display">
```

Área donde se muestra el rango seleccionado.

Ejemplo:

```
12 junio 2026 - 18 junio 2026
```

---

### Popup de selección

```html
<div class="plus-select-date-popup">
```

Es el **panel desplegable** que contiene:

* opciones rápidas
* selector personalizado

---

# Opciones rápidas de fecha

```html
<div data-range="today">
<div data-range="7days">
<div data-range="thismonth">
<div data-range="lastmonth">
<div data-range="custom">
```

Opciones disponibles:

| opción    | significado         |
| --------- | ------------------- |
| today     | hoy                 |
| 7days     | últimos 7 días      |
| thismonth | mes actual          |
| lastmonth | mes anterior        |
| custom    | rango personalizado |

---

# Selección personalizada

```html
<div class="plus-select-date-custom">
```

Permite seleccionar fechas manualmente.

Contiene:

```html
<input type="date" class="date-start">
<input type="date" class="date-end">
```

---

# Botones de acción

```html
<button class="btn-accept">
<button class="btn-cancel">
```

Funciones:

| botón    | acción        |
| -------- | ------------- |
| Aceptar  | aplicar rango |
| Cancelar | cerrar popup  |

---

# Inputs ocultos

```html
<input type="hidden">
```

Se crean dos inputs ocultos:

```html
<input id="fecha_inicio">
<input id="fecha_fin">
```

Estos se utilizan para **enviar los valores en formularios**.

Ejemplo de valores guardados:

```
2026-06-12
2026-06-18
```

---

# Selección de elementos del DOM

Después de crear el HTML se obtienen las referencias.

```javascript
const display
const popup
const options
const customDiv
const dateStart
const dateEnd
const btnAccept
const btnCancel
```

Esto permite manipular los elementos.

---

# formatDate()

```javascript
function formatDate(date)
```

Convierte un objeto `Date` a una cadena legible.

Ejemplo:

```
12 June 2026
```

Usa:

```javascript
date.toLocaleDateString()
```

---

# setRange()

```javascript
function setRange(start, end)
```

Este método:

1. Actualiza el texto mostrado
2. Guarda los valores en los inputs ocultos

Código clave:

```javascript
rangeSpan.textContent = "fecha_inicio - fecha_fin"
```

y

```javascript
inputStart.value
inputEnd.value
```

---

# Evento para abrir el popup

```javascript
display.addEventListener('click')
```

Al hacer clic se muestra el panel.

```javascript
popup.classList.toggle('active')
```

---

# Eventos de opciones rápidas

Cada opción tiene un evento.

```javascript
options.forEach(option => {
  option.addEventListener('click')
})
```

Dependiendo del tipo seleccionado se calcula el rango.

---

# Cálculo de rangos

## Hoy

```javascript
start = end = now
```

---

## Últimos 7 días

```javascript
end = now
start = now - 6 días
```

---

## Mes actual

```javascript
start = primer día del mes
end = último día del mes
```

---

## Mes anterior

```javascript
start = primer día del mes anterior
end = último día del mes anterior
```

---

# Rango personalizado

Si el usuario selecciona:

```
custom
```

Se activa la sección:

```javascript
customDiv.classList.add('active')
```

---

# Botón aceptar

```javascript
btnAccept.addEventListener('click')
```

Este botón:

1. obtiene las fechas seleccionadas
2. crea objetos `Date`
3. llama a `setRange()`

---

# Botón cancelar

```javascript
btnCancel.addEventListener('click')
```

Solo cierra el popup.

---

# Cierre automático del popup

```javascript
document.addEventListener('click')
```

Si el usuario hace clic fuera del componente:

```
popup se cierra
```

Esto mejora la experiencia de usuario.

---

# Ejemplo de uso

```html
<plus-select-date
  name-start="date_from"
  name-end="date_to"
  label="Fecha"
  required>
</plus-select-date>
```

Resultado:

```
Fecha *
[ seleccionar rango ]
```

---

# Valores enviados en formulario

Cuando el usuario selecciona:

```
1 Junio 2026 - 10 Junio 2026
```

Los inputs ocultos tendrán:

```
date_from = 2026-06-01
date_to   = 2026-06-10
```

---

# Ventajas del componente

`PlusSelectDate` ofrece varias ventajas:

* selector de fechas moderno
* opciones rápidas de rango
* selección personalizada
* soporte de traducciones
* integración con formularios
* encapsulación con Shadow DOM
* interfaz limpia y reutilizable

---

# Conclusión

`PlusSelectDate` es un **componente web avanzado para seleccionar rangos de fechas**, diseñado para ser reutilizable, traducible y fácil de integrar en formularios y dashboards.

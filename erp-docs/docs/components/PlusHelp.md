# PlusHelp

`PlusHelp` es un **Web Component personalizado** que permite crear un **sistema de ayuda interactiva paso a paso dentro de una aplicación web**.

Este componente muestra un **botón de ayuda** que, al hacer clic, abre una ventana emergente con:

* pasos explicativos
* texto de ayuda
* imágenes
* videos de YouTube
* navegación entre pasos
* barra de progreso

En resumen:

**`PlusHelp` = un sistema de ayuda interactiva integrado en la interfaz.**

---

# Código base

```javascript
class PlusHelp extends HTMLElement
```

Esto significa que el componente **extiende un elemento HTML nativo** usando la API de **Web Components**.

Después de registrarlo:

```javascript
customElements.define("plus-help", PlusHelp);
```

puede utilizarse directamente en HTML.

Ejemplo:

```html
<plus-help data="invoice-help"></plus-help>
```

---

# Propósito del componente

El objetivo de `PlusHelp` es **explicar al usuario cómo usar una parte del sistema** mediante una guía paso a paso.

Esto es útil para:

* onboarding de usuarios
* tutoriales
* explicar funcionalidades complejas
* documentación dentro de la aplicación

---

# Estructura general del componente

El componente tiene tres partes principales:

```
constructor()
connectedCallback()
métodos internos
```

---

# constructor()

```javascript
constructor() {
  super();
  this.currentStep = 0;
}
```

El constructor inicializa el estado interno del componente.

## currentStep

```javascript
this.currentStep = 0;
```

Guarda **el paso actual de la guía** que se está mostrando.

Ejemplo:

```
0 → Paso 1
1 → Paso 2
2 → Paso 3
```

---

# connectedCallback()

```javascript
connectedCallback()
```

Este método se ejecuta **automáticamente cuando el componente se agrega al DOM**.

Aquí se crean:

* el botón de ayuda
* el popup de ayuda
* los eventos de interacción

---

# Obtener título del botón

Primero se obtiene el título del botón.

```javascript
const title = this.getAttribute('title') || window.t('info.help');
```

Si el programador no define un título, se usa uno por defecto.

Ejemplo:

```html
<plus-help title="Help"></plus-help>
```

---

# Soporte de traducciones

El componente permite traducir el texto automáticamente.

```javascript
const t = this.getAttribute('t') || title;
const titleTranslate = window.translate_text(t);
```

Esto permite usar:

```html
<plus-help t="help.invoice"></plus-help>
```

---

# Obtener clave de datos

```javascript
const dataKey = this.getAttribute('data') || 'default-help';
```

Esta clave define **qué contenido de ayuda cargar**.

Ejemplo:

```html
<plus-help data="invoice-help"></plus-help>
```

Luego el componente buscará:

```javascript
window.PLUS_HELP_DATA["invoice-help"]
```

---

# Crear botón de ayuda

Se crea un botón HTML.

```javascript
const button = document.createElement('button');
```

Configuración del botón:

```
type="button"
texto del título
clase CSS
```

```javascript
button.classList.add('plus-help-button');
```

---

# Crear ventana emergente (popup)

El popup es un contenedor donde se mostrará la ayuda.

```javascript
const popup = document.createElement('div');
popup.classList.add('plus-help-popup', 'plus-help-hidden');
```

La clase `plus-help-hidden` mantiene el popup oculto inicialmente.

---

# Estructura interna del popup

El popup contiene:

```
header
barra de progreso
contenido del paso
footer con navegación
```

Estructura:

```
plus-help-popup
 ├ header
 ├ progress
 ├ step-content
 └ footer
```

---

# Header

```html
<div class="plus-help-header">
```

Contiene:

* título
* botón para cerrar

```html
<button class="plus-help-close">X</button>
```

---

# Barra de progreso

```html
<div class="plus-help-progress"></div>
```

Muestra visualmente **qué paso se está viendo**.

Ejemplo:

```
[1] [2] [3] [4]
```

---

# Contenedor de pasos

```html
<div class="plus-help-step-container"></div>
```

Aquí se insertará el contenido de cada paso.

---

# Footer de navegación

El footer contiene:

```
botón anterior
estado actual
botón siguiente
```

Ejemplo:

```
← Previous   Step 1 of 5   Next →
```

---

# Evento para abrir la ayuda

Cuando el usuario hace clic en el botón:

```javascript
button.addEventListener('click', () => {
```

Se ejecuta:

```
mostrar popup
cargar contenido
```

Código:

```javascript
popup.classList.remove('plus-help-hidden');
this.loadHelpContent(dataKey);
```

---

# Evento para cerrar la ayuda

El botón **X** cierra el popup.

```javascript
popup.querySelector('.plus-help-close').addEventListener('click', () => {
```

Acciones:

```
ocultar popup
reiniciar pasos
```

Código:

```javascript
popup.classList.add('plus-help-hidden');
this.currentStep = 0;
```

---

# Insertar elementos en el DOM

Finalmente se agregan al componente.

```javascript
this.appendChild(button);
this.appendChild(popup);
```

Resultado final:

```
<plus-help>
   button
   popup
</plus-help>
```

---

# loadHelpContent()

```javascript
loadHelpContent(key)
```

Este método carga los datos de ayuda desde:

```
window.PLUS_HELP_DATA
```

Ejemplo de estructura:

```javascript
window.PLUS_HELP_DATA = {
  "invoice-help": {
    steps: [
      {
        title: "Create invoice",
        text: "Click the create button"
      }
    ]
  }
}
```

Código:

```javascript
this.helpData = window.PLUS_HELP_DATA?.[key]?.steps || [];
```

También se calcula:

```javascript
this.totalSteps = this.helpData.length;
```

Luego se renderiza el primer paso.

```javascript
this.renderStep();
```

---

# renderStep()

```javascript
renderStep()
```

Este método **muestra el paso actual**.

---

# Obtener elementos del popup

Se obtienen referencias a los elementos del DOM.

```javascript
const content = this.querySelector('#plus-help-content');
```

También:

```
progress
status
prevBtn
nextBtn
```

---

# Verificar si existe ayuda

Si no hay datos:

```javascript
if (!this.helpData.length)
```

Se muestra un mensaje:

```
No help information available
```

---

# Obtener paso actual

```javascript
const step = this.helpData[this.currentStep];
```

Ejemplo:

```
Paso 1
Paso 2
Paso 3
```

---

# Mostrar contenido del paso

El contenido puede incluir:

```
título
texto
imagen
video
```

Ejemplo generado:

```html
<div class="plus-help-step">
  <h3>Título</h3>
  <p>Texto</p>
</div>
```

---

# Soporte para imágenes

Si el paso tiene imagen:

```javascript
step.image
```

Se agrega:

```html
<img class="plus-help-img">
```

---

# Soporte para video

Si el paso tiene video:

```javascript
step.video
```

Se crea un iframe de YouTube.

```html
<iframe src="https://www.youtube.com/embed/...">
```

---

# Actualizar barra de progreso

Se genera una lista de pasos.

Ejemplo visual:

```
1 2 3 4
```

Código:

```javascript
progress.innerHTML = this.helpData.map(...)
```

Clases utilizadas:

```
active
done
```

---

# Actualizar estado

Se muestra:

```
Step 1 of 5
```

Código:

```javascript
status.textContent = `${textStep} ${this.currentStep + 1} ${textOF} ${this.totalSteps}`;
```

---

# Botón anterior

```javascript
prevBtn.onclick
```

Reduce el paso actual.

```javascript
this.currentStep--;
this.renderStep();
```

---

# Botón siguiente

```javascript
nextBtn.onclick
```

Avanza al siguiente paso.

```javascript
this.currentStep++;
this.renderStep();
```

---

# Deshabilitar botones

Se deshabilitan cuando corresponde.

Ejemplo:

```
Paso 1 → no se puede ir atrás
Último paso → no se puede avanzar
```

Código:

```javascript
prevBtn.disabled = this.currentStep === 0;
nextBtn.disabled = this.currentStep === this.totalSteps - 1;
```

---

# Lazy loading de imágenes

Si existe la librería `loadzy`:

```javascript
if (window.loadzy) loadzy();
```

Esto permite **cargar imágenes solo cuando se necesitan**, mejorando el rendimiento.

---

# Ejemplo completo

HTML:

```html
<plus-help
title="Help"
data="invoice-help">
</plus-help>
```

---

JavaScript:

```javascript
window.PLUS_HELP_DATA = {
  "invoice-help": {
    steps: [
      {
        title: "Create invoice",
        text: "Click the create button"
      },
      {
        title: "Add products",
        text: "Select products in the table"
      }
    ]
  }
};
```

---

# Flujo de funcionamiento

```
plus-help se agrega al DOM
        ↓
connectedCallback()
        ↓
crear botón de ayuda
        ↓
crear popup oculto
        ↓
usuario hace clic
        ↓
cargar datos de ayuda
        ↓
mostrar paso actual
        ↓
navegar entre pasos
```

---

# Ventajas del componente

Usar `PlusHelp` tiene varias ventajas:

* guía interactiva dentro de la aplicación
* fácil de reutilizar
* soporta traducciones
* soporta imágenes
* soporta videos
* navegación paso a paso
* barra de progreso
* mejora la experiencia del usuario

---

# Conclusión

`PlusHelp` es un **componente de ayuda interactiva basado en Web Components** que permite integrar tutoriales paso a paso dentro de una aplicación web.

Proporciona una forma clara y visual de **explicar funcionalidades al usuario directamente dentro del sistema**, mejorando la usabilidad y reduciendo la necesidad de documentación externa.

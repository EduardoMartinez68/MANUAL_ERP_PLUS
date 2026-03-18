# PlusPanel Component

`PlusPanel` es un **Web Component de PLUS UI** que permite crear paneles laterales deslizables dentro del ERP.

Este componente funciona como un **side panel o drawer**, utilizado para mostrar formularios, configuraciones o información adicional sin cambiar de página.

El panel puede aparecer desde la izquierda o la derecha y soporta:

- overlay de fondo
- efecto blur
- tamaño configurable
- cierre automático al hacer clic fuera del panel
- contenido dinámico

---

## Uso básico

Para utilizar el panel solo se necesita la etiqueta personalizada.

```html
<plus-panel 
    side="right"
    size="500px"
    title="User settings">

    <p>Panel content here</p>

</plus-panel>
```

El contenido dentro del componente se renderiza automáticamente dentro del panel.

---

## Atributos

| Atributo | Descripción |
|---|---|
| `title` | Título mostrado en la barra superior |
| `side` | Lado desde el que aparece el panel (`left` o `right`) |
| `size` | Ancho del panel |
| `blur-background` | Activa el efecto blur en el fondo |
| `close-on-overlay` | Permite cerrar el panel al hacer clic fuera |

---

## Ejemplo completo

```html
<plus-panel
    id="userPanel"
    side="right"
    size="450px"
    title="User profile"
    blur-background="true"
    close-on-overlay="true">

    <h3>User information</h3>

    <input type="text" placeholder="Name">

    <button onclick="saveUser()">
        Save
    </button>

</plus-panel>
```

---

## Mostrar el panel

El panel se controla mediante métodos JavaScript.

```javascript
const panel = document.querySelector("#userPanel");

panel.show();
```

---

## Ocultar el panel

```javascript
panel.hide();
```

---

## Cambiar tamaño dinámicamente

El tamaño del panel puede modificarse usando porcentaje del viewport.

```javascript
panel.setSize(60);
```

Esto configurará el panel al **60% del ancho de la pantalla**.

---

## Cambiar el título

```javascript
panel.setTitle("Edit user");
```

El texto se procesa automáticamente con el sistema de traducción del ERP.

---

## Cambiar el lado del panel

```javascript
panel.setSide("left");
```

Opciones válidas:

- `left`
- `right`

---

## Activar o desactivar blur

```javascript
panel.setBlur(true);
```

Para desactivarlo:

```javascript
panel.setBlur(false);
```

---

## Actualizar múltiples opciones

Es posible modificar varias propiedades al mismo tiempo.

```javascript
panel.updateOptions({
    size: 70,
    title: "Settings",
    side: "right",
    blur: true
});
```

---

## Ejemplo en interfaz ERP

```html
<button onclick="document.querySelector('#settingsPanel').show()">
    Open settings
</button>

<plus-panel
    id="settingsPanel"
    title="System settings"
    side="right"
    size="500px"
    blur-background="true">

    <h3>Configuration</h3>

    <p>System configuration options.</p>

</plus-panel>
```

---

## Funcionamiento interno

Cuando el componente se conecta al DOM:

1. Lee los atributos del componente
2. Genera un **overlay de pantalla completa**
3. Crea el contenedor del panel
4. Inserta el contenido usando un `slot`
5. Aplica estilos mediante **Shadow DOM**

Flujo interno:

```
HTML
 ↓
plus-panel
 ↓
Web Component
 ↓
Shadow DOM
 ↓
Overlay + Panel container
```

---

## Código del componente

```javascript
class PlusPanel extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
  }

  connectedCallback() {

    const side = this.getAttribute('side') || 'left';
    const size = this.getAttribute('size') || '400px';

    const overlay = document.createElement('div');
    const container = document.createElement('div');

    const slot = document.createElement('slot');
    container.appendChild(slot);

    overlay.appendChild(container);
    this.shadowRoot.appendChild(overlay);

  }

}
```

---

## Registro del componente

Para usar el componente debe registrarse en el sistema de **Custom Elements**.

```javascript
customElements.define("plus-panel", PlusPanel);
```

---

## Ventajas dentro de PLUS UI

`PlusPanel` permite crear interfaces modernas dentro del ERP sin necesidad de frameworks externos.

Beneficios:

- paneles dinámicos
- carga rápida
- integración directa con PLUS UI
- reutilización de componentes
- experiencia de usuario moderna

---

## Integración con PLUS UI

`PlusPanel` forma parte del sistema de componentes de **PLUS UI**.

Otros componentes del sistema incluyen:

- `info-label`
- `message-pop`
- `plus-panel`
- `erp-button`
- `erp-table`

Estos componentes permiten construir interfaces complejas manteniendo un código limpio y reutilizable.
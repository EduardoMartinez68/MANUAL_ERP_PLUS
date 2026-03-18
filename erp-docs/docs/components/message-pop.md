# MessagePop Component

`MessagePop` es un **Web Component de PLUS UI** que permite mostrar ventanas emergentes (popups o modales) de forma sencilla dentro del ERP.

Este componente encapsula toda la lógica necesaria para renderizar un cuadro de mensaje con:

- título
- contenido dinámico
- botón de cierre
- tamaño configurable

El objetivo es simplificar la creación de ventanas emergentes dentro del sistema.

---

## Uso básico

Para crear un popup solo es necesario usar la etiqueta personalizada.

```html
<message-pop 
    name="welcome-pop"
    title="Welcome"
    width="500px"
    height="300px">

    Welcome to PLUS ERP.

</message-pop>
```

El componente generará automáticamente la estructura HTML necesaria para mostrar la ventana.

---

## Atributos

| Atributo | Descripción |
|--------|-------------|
| `name` | Identificador único del popup |
| `title` | Título que aparecerá en la parte superior |
| `width` | Ancho del popup |
| `height` | Alto del popup |

---

## Ejemplo con contenido HTML

El componente puede recibir contenido HTML complejo.

```html
<message-pop 
    name="delete-confirm"
    title="Confirm delete"
    width="500px"
    height="250px">

    <p>Are you sure you want to delete this record?</p>

    <button class="btn btn-danger">
        Delete
    </button>

</message-pop>
```

---

## Resultado generado

El componente transforma la etiqueta personalizada en la siguiente estructura:

```html
<div id="delete-confirm" class="my-pop">

  <div class="my-pop-content-wrapper">

    <div class="my-pop-header">

      <h4 class="my-pop-title">
        Confirm delete
      </h4>

      <button class="close-btn">×</button>

    </div>

    <div class="my-pop-content">

        <p>Are you sure you want to delete this record?</p>

        <button class="btn btn-danger">
            Delete
        </button>

    </div>

  </div>

</div>
```

---

## Funcionamiento interno

Cuando el componente se conecta al DOM (`connectedCallback`):

1. Guarda el contenido original del elemento.
2. Obtiene los atributos definidos por el desarrollador.
3. Traduce el título usando `translate_text()`.
4. Genera dinámicamente el HTML del popup.
5. Inserta el contenido dentro del contenedor `.my-pop-content`.

Flujo de ejecución:

```
HTML personalizado
       ↓
message-pop
       ↓
Web Component
       ↓
render()
       ↓
Generación de popup dinámico
```

---

## Código del componente

```javascript
class MessagePop extends HTMLElement {

  constructor() {
    super();
    this._template = "";
    this._initialized = false;
  }

  connectedCallback() {

    if (!this._initialized) {

      this._template = this.innerHTML.trim();
      this._initialized = true;

      this.render();
    }

  }

  render() {

    const title = this.getAttribute('title') || '';
    const translatedTitle = window.translate_text(title);

    const name = this.getAttribute('name') || 'pop-default';

    const width = this.getAttribute('width') || '600px';
    const height = this.getAttribute('height') || '400px';

    const content = this.innerHTML.trim();

    const wrappedContent = content.startsWith('<')
        ? content
        : `<p>${content}</p>`;

    this.innerHTML = `
        <div id="${name}" class="my-pop">

          <div class="my-pop-content-wrapper">

            <div class="my-pop-header">

              <h4 class="my-pop-title" t="${translatedTitle}">
                ${translatedTitle}
              </h4>

              <button class="close-btn"
                onclick="close_my_pop('${name}')"
                type="button">

                ×

              </button>

            </div>

            <div class="my-pop-content">

              ${wrappedContent}

            </div>

          </div>

        </div>
    `;

  }

}
```

---

## Registro del componente

Para usar el componente es necesario registrarlo en el sistema de **Custom Elements**.

```javascript
customElements.define("message-pop", MessagePop);
```

---

## Ventajas

El componente `MessagePop` ofrece varias ventajas dentro de PLUS UI.

- Simplifica la creación de popups
- Reduce código repetitivo
- Permite contenido dinámico
- Facilita la traducción de textos
- Mantiene consistencia visual en el ERP

---

## Integración con PLUS UI

`MessagePop` forma parte del sistema de componentes **PLUS UI**, que proporciona elementos reutilizables para construir la interfaz del ERP de forma rápida y consistente.

Otros componentes relacionados:

- `info-label`
- `erp-button`
- `erp-modal`
- `erp-table`

Cada componente encapsula lógica específica para mejorar la productividad del desarrollo frontend.
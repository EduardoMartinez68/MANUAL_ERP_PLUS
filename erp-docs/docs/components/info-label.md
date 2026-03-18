# InfoLabel

`InfoLabel` es un **Web Component** que genera automáticamente una etiqueta (`label`) con un icono de ayuda y un tooltip informativo.

Este componente se utiliza para mostrar información contextual en formularios del ERP.

---

## Uso básico

El programador solo necesita usar la etiqueta personalizada en HTML.

```html
<info-label 
    label="product_name" 
    message="This is the name of the product">
</info-label>
```

Resultado generado:

```
Nombre del producto (?) 
```

Al pasar el cursor sobre el icono aparecerá un tooltip con la explicación.

---

## Atributos

| Atributo | Descripción |
|--------|-------------|
| `label` | Clave del texto que se mostrará como etiqueta |
| `message` | Texto informativo que se mostrará en el tooltip |
| `help` | Campo opcional para futuras extensiones |

---

## Ejemplo completo

```html
<div class="form-group">

    <info-label 
        label="customer_name"
        message="Enter the full name of the customer">
    </info-label>

    <input type="text" class="form-control">

</div>
```

---

## Comportamiento

Cuando el componente se monta en el DOM:

1. Verifica si existe un tooltip global `.erp-tooltip`.
2. Si no existe, lo crea.
3. Traduce el texto usando `translate_text()`.
4. Genera un `label` con:
   - texto
   - icono de ayuda
5. Agrega eventos `mouseenter`, `mousemove` y `mouseleave`.

---

## Código fuente

```javascript
class InfoLabel extends HTMLElement {

  constructor() {
    super();
  }

  connectedCallback() {

    if (!document.querySelector('.erp-tooltip')) {

      const tooltip = document.createElement("div");

      tooltip.className = "erp-tooltip";
      tooltip.style.position = "absolute";
      tooltip.style.pointerEvents = "none";
      tooltip.style.opacity = "0";
      tooltip.style.transition = "opacity 0.3s ease";

      document.body.appendChild(tooltip);
    }

    const tooltip = document.querySelector('.erp-tooltip');

    const keyLabelText = this.getAttribute("label") || "";
    const keyMessage = this.getAttribute("message") || "";

    const labelText = translate_text(keyLabelText);
    const message = translate_text(keyMessage);

    const label = document.createElement("label");

    const span = document.createElement("span");
    span.textContent = labelText;

    const icon = document.createElement("i");
    icon.className = "fi fi-sr-interrogation";

    icon.addEventListener("mouseenter", () => {
      tooltip.textContent = message;
      tooltip.style.opacity = "1";
    });

    icon.addEventListener("mouseleave", () => {
      tooltip.style.opacity = "0";
    });

    label.appendChild(span);
    label.appendChild(icon);

    this.replaceWith(label);
  }

}
```

---

## Registro del componente

Para usar el componente es necesario registrarlo en JavaScript.

```javascript
customElements.define("info-label", InfoLabel);
```

---

## Flujo del componente

```
HTML
 ↓
info-label
 ↓
Web Component
 ↓
Genera <label>
 ↓
Agrega tooltip dinámico
```

---

## Ventajas

- Simplifica el HTML de formularios
- Permite tooltips consistentes
- Facilita internacionalización
- Reduce duplicación de código
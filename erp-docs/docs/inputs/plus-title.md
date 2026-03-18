# PlusTitle

`PlusTitle` es un **Web Component** que genera automáticamente un **label + input** para títulos o nombres principales dentro de formularios del ERP.

Este componente simplifica la creación de campos de título, manteniendo **estructura consistente**, **soporte para traducciones** y **IDs únicos automáticos**.

---

## Uso básico

El programador solo necesita usar la etiqueta personalizada en HTML.

```html
<plus-title 
    label="case_title" 
    placeholder="Enter title">
</plus-title>
```

Resultado generado:

```
Case Title
[_____________________]
```

El componente generará automáticamente un `label` y un `input`.

---

## Atributos

| Atributo        | Descripción                                                  |
| --------------- | ------------------------------------------------------------ |
| `label`         | Texto del label que se mostrará                              |
| `placeholder`   | Texto placeholder del input                                  |
| `t-placeholder` | Clave de traducción para el placeholder                      |
| `id`            | ID del input. Si no se proporciona se genera automáticamente |
| `name`          | Nombre del campo para formularios                            |
| `value`         | Valor inicial del input                                      |
| `type`          | Tipo de input (text, number, etc.)                           |

---

## Ejemplo completo

```html
<div class="form-group">

    <plus-title 
        label="Case title"
        name="case_title"
        placeholder="Enter case title">
    </plus-title>

</div>
```

HTML generado:

```html
<label t="Case title">Case title</label>
<input 
    class="case-title-input"
    id="generated_id"
    name="case_title"
    placeholder="Enter case title">
```

---

## Comportamiento

Cuando el componente se monta en el DOM:

1. Obtiene el atributo `label`.
2. Crea un elemento `label`.
3. Asigna el atributo `t` al label para soporte de traducción.
4. Genera un `input` con la clase `case-title-input`.
5. Si no se proporciona un `id`, genera uno automáticamente usando `generate_unique_dom_id()`.
6. Copia todos los atributos del componente al `input`.
7. Reemplaza el elemento `<plus-title>` con:

```
<label>
<input>
```

---

## Código fuente

```javascript
class PlusTitle extends HTMLElement {
  constructor() {
    super();
  }

  connectedCallback() {
    // Texto del label = contenido del componente
    const labelText = this.getAttribute('label');

    // Creamos label
    const label = document.createElement("label");
    if (this.hasAttribute("label")) {
      label.setAttribute("label", this.getAttribute("label"));
    }
    label.setAttribute('t', labelText)
    label.textContent = labelText;

    // Creamos input
    const id = this.getAttribute('id') || generate_unique_dom_id();
    const input = document.createElement("input");
    input.classList.add("case-title-input");
    input.setAttribute('t-placeholder', this.getAttribute('t-placeholder') || this.getAttribute('placeholder') || '');
    input.setAttribute('id', id);

    // Pasamos todos los atributos menos "class"
    [...this.attributes].forEach(attr => {
      if (attr.name !== "class") {
        input.setAttribute(attr.name, attr.value);
      }
    });

    // Reemplazamos <plus-title> con label + input
    const wrapper = document.createDocumentFragment();
    wrapper.appendChild(label);
    wrapper.appendChild(input);

    this.replaceWith(wrapper);
  }
}
```

---

## Registro del componente

Para utilizar el componente es necesario registrarlo en JavaScript.

```javascript
customElements.define("plus-title", PlusTitle);
```

---

## Flujo del componente

```
HTML
 ↓
plus-title
 ↓
Web Component
 ↓
Genera <label>
 ↓
Genera <input>
 ↓
Reemplaza el componente
```

---

## Ventajas

* Reduce la cantidad de HTML en formularios
* Mantiene consistencia en títulos de formularios
* Soporta traducciones (`t` y `t-placeholder`)
* Genera IDs automáticamente
* Permite reutilización en todo el ERP

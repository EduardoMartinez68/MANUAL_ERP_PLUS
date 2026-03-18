# SearchBar

`SearchBar` es un **Web Component** que genera automáticamente una **barra de búsqueda con icono**, diseñada para filtrar información dentro del ERP.

Este componente incluye **debounce automático**, integración con **funciones JavaScript personalizadas** y soporte para **traducciones**.

Permite ejecutar una función cuando el usuario escribe o presiona **Enter** en el campo de búsqueda.

---

## Uso básico

El programador solo necesita usar la etiqueta personalizada en HTML.

```html
<search-bar 
    name="search"
    placeholder="Search...">
</search-bar>
```

Resultado generado:

```
[ 🔍  Search... ]
```

El componente crea automáticamente un **input de búsqueda con icono**.

---

## Atributos

| Atributo            | Descripción                                                  |
| ------------------- | ------------------------------------------------------------ |
| `name`              | Nombre del campo del formulario                              |
| `label`             | Texto del label asociado a la búsqueda                       |
| `placeholder`       | Texto que aparece dentro del input                           |
| `value`             | Valor inicial del input                                      |
| `id`                | ID del input. Si no se define se genera automáticamente      |
| `required`          | Marca el campo como obligatorio                              |
| `readonly`          | Hace el campo solo lectura                                   |
| `disabled`          | Desactiva el campo                                           |
| `function-callback` | Nombre de la función JavaScript que se ejecutará al escribir |

---

## Ejemplo completo

```html
<div class="form-group">

    <search-bar
        name="customer_search"
        label="Search customer"
        placeholder="Search customer..."
        function-callback="searchCustomers">
    </search-bar>

</div>
```

---

## Callback de búsqueda

El atributo `function-callback` permite ejecutar una función JavaScript cada vez que el usuario escribe en el campo.

Ejemplo:

```javascript
function searchCustomers(text) {
    console.log("Searching:", text);
}
```

El componente enviará el texto escrito por el usuario a esta función.

---

## Sistema de debounce

Para evitar ejecutar demasiadas búsquedas mientras el usuario escribe, el componente utiliza **debounce**.

Configuración interna:

```
500 ms
```

Esto significa que la función se ejecutará **500 milisegundos después de que el usuario deje de escribir**.

También se ejecuta inmediatamente cuando el usuario presiona **Enter**.

---

## HTML generado

El componente genera internamente una estructura similar a:

```html
<div class="search-wrapper">

  <input 
      type="search"
      class="search-input">

  <i class="fi fi-br-search search-icon"></i>

</div>
```

---

## Comportamiento

Cuando el componente se monta en el DOM:

1. Obtiene los atributos del componente (`name`, `placeholder`, `value`, etc).
2. Crea un contenedor `div` con la clase `search-wrapper`.
3. Genera un `input` tipo `search`.
4. Agrega un icono de búsqueda.
5. Aplica traducción al placeholder usando `translate_text`.
6. Si se define `function-callback`, registra eventos para ejecutar la función.
7. Aplica **debounce** a los eventos de escritura.
8. Ejecuta la función inmediatamente si el usuario presiona **Enter**.
9. Reemplaza el elemento `<search-bar>` con el HTML generado.

---

## Código fuente

```javascript
class SearchBar extends HTMLElement {
  constructor() {
    super();
    this._typingTimer = null;
    this._doneTypingInterval = 500;
  }

  connectedCallback() {
    const labelTextInfo = this.getAttribute("label") || "";
    const labelText = labelTextInfo || "message.search";
    const name = this.getAttribute("name") || "";
    const placeHolderText = this.getAttribute("placeholder") || "";
    const required = this.hasAttribute("required");
    const readOnly = this.hasAttribute("readonly");
    const disabled = this.hasAttribute("disabled");
    const value = this.getAttribute("value") || "";

    const placeholder = placeHolderText || "message.search";
    const tPlaceholderKey = placeHolderText || "message.search";

    const wrapper = document.createElement("div");
    wrapper.classList.add("search-wrapper");

    const label = document.createElement("label");
    label.setAttribute('t', labelText);
    label.textContent = window.translate_text(labelTextInfo);

    const input = document.createElement("input");
    input.type = "search";
    input.classList.add("search-input");
    input.name = name;
    input.id = this.getAttribute("id") || generate_unique_dom_id();
    input.placeholder = window.translate_text(placeholder);
    input.value = value;
    input.setAttribute("t-placeholder", tPlaceholderKey);

    if (required) input.required = true;
    if (readOnly) input.readOnly = true;
    if (disabled) input.disabled = true;

    const icon = document.createElement("i");
    icon.className = "fi fi-br-search search-icon";

    wrapper.appendChild(input);
    wrapper.appendChild(icon);

    const callbackFnName = this.getAttribute("function-callback");

    if (callbackFnName && typeof window[callbackFnName] === "function") {

      input.addEventListener('input', () => {
        clearTimeout(this._typingTimer);
        this._typingTimer = setTimeout(() => {
          const currentText = input.value;
          window[callbackFnName](currentText);
        }, this._doneTypingInterval);
      });

      input.addEventListener('keydown', (e) => {
        if (e.key === 'Enter') {
          clearTimeout(this._typingTimer);
          window[callbackFnName](input.value);
        }
      });
    }

    this.replaceWith(wrapper);
  }
}
```

---

## Registro del componente

Para usar el componente es necesario registrarlo en JavaScript.

```javascript
customElements.define("search-bar", SearchBar);
```

---

## Flujo del componente

```
HTML
 ↓
search-bar
 ↓
Web Component
 ↓
Genera input + icono
 ↓
Escucha eventos de escritura
 ↓
Ejecuta callback con debounce
```

---

## Ventajas

* Simplifica la creación de buscadores en el ERP
* Evita repetir código HTML y JavaScript
* Incluye sistema de **debounce automático**
* Permite ejecutar funciones personalizadas
* Soporte para internacionalización
* Interfaz consistente en todas las vistas

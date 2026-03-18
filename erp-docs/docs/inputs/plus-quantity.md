# PlusQuantity

`PlusQuantity` es un **Web Component** que crea un **selector de cantidad interactivo** con botones para incrementar y disminuir el valor.

Este componente está diseñado para formularios del ERP y utiliza **Form Associated Custom Elements**, lo que permite que funcione como un **input nativo dentro de formularios HTML**.

Incluye:

* Botones `+` y `−`
* Validación automática de límites
* Control de pasos (`step`)
* Integración con formularios (`formAssociated`)
* UI encapsulada con **Shadow DOM**

---

## Uso básico

El programador solo necesita usar la etiqueta personalizada en HTML.

```html
<plus-quantity 
    name="quantity"
    label="Quantity"
    value="1">
</plus-quantity>
```

Resultado generado:

```
Quantity        (-)  1  (+)
```

El componente mostrará un selector de cantidad con botones para aumentar o disminuir el valor.

---

## Atributos

| Atributo | Descripción                                   |
| -------- | --------------------------------------------- |
| `name`   | Nombre del campo para formularios             |
| `label`  | Texto que aparece a la izquierda del selector |
| `value`  | Valor inicial                                 |
| `min`    | Valor mínimo permitido                        |
| `max`    | Valor máximo permitido                        |
| `step`   | Incremento o decremento del valor             |

---

## Ejemplo completo

```html
<div class="form-group">

    <plus-quantity
        name="product_quantity"
        label="Product quantity"
        value="2"
        min="1"
        max="50"
        step="1">
    </plus-quantity>

</div>
```

---

## HTML generado internamente

El componente crea internamente una interfaz similar a:

```html
<div class="container">

  <span class="label">Product quantity</span>

  <div class="controls">

    <button>-</button>

    <input type="number">

    <button>+</button>

  </div>

</div>
```

Todo el HTML y CSS se encapsula dentro del **Shadow DOM**, evitando conflictos con estilos globales.

---

## Comportamiento

Cuando el componente se monta en el DOM:

1. Crea un **Shadow DOM** para encapsular la interfaz.
2. Genera el contenedor visual con:

   * Label
   * Botón de disminuir
   * Input numérico
   * Botón de aumentar
3. Inicializa el valor usando el atributo `value`.
4. Registra el valor dentro del formulario usando `ElementInternals`.
5. Aplica las restricciones `min`, `max` y `step`.
6. Desactiva visualmente los botones cuando se alcanzan los límites.
7. Escucha eventos para actualizar el valor cuando el usuario:

   * Hace clic en `+`
   * Hace clic en `−`
   * Cambia el valor manualmente en el input.

---

## Integración con formularios

El componente utiliza:

```
static formAssociated = true
```

Esto permite que el valor del componente sea enviado automáticamente en formularios HTML.

Ejemplo:

```html
<form>

    <plus-quantity 
        name="quantity"
        label="Quantity"
        value="3">
    </plus-quantity>

    <button type="submit">Send</button>

</form>
```

El formulario enviará:

```
quantity = 3
```

---

## Código fuente

```javascript
class PlusQuantity extends HTMLElement {
  static formAssociated = true;

  constructor() {
    super();
    this.internals = this.attachInternals();
    this.attachShadow({ mode: 'open' });
  }

  connectedCallback() {
    this.render();
  }

  static get observedAttributes() {
    return ['value', 'min', 'max', 'step', 'label', 'name'];
  }

  get value() { return this.getAttribute('value') || 0; }

  set value(val) {
    const min = parseFloat(this.getAttribute('min')) || -Infinity;
    const max = parseFloat(this.getAttribute('max')) || Infinity;

    let newValue = Math.max(min, Math.min(max, parseFloat(val) || min));
    
    this.setAttribute('value', newValue);
    if (this.input) this.input.value = newValue;
    this.internals.setFormValue(newValue);

    this.updateButtonState();
  }

  updateValue(delta) {
    const step = parseFloat(this.getAttribute('step')) || 1;
    this.value = parseFloat(this.value) + (delta * step);
  }

  updateButtonState() {
    if(!this.btnMinus || !this.btnPlus) return;

    const min = parseFloat(this.getAttribute('min')) || -Infinity;
    const max = parseFloat(this.getAttribute('max')) || Infinity;
    const currentVal = parseFloat(this.value);

    this.btnMinus.classList.toggle('disabled', currentVal <= min);
    this.btnPlus.classList.toggle('disabled', currentVal >= max);
  }

  render() {
    const label = this.getAttribute('label') || 'Cantidad';
    const min = this.getAttribute('min') || 0;
    const max = this.getAttribute('max') || 100;
    const step = this.getAttribute('step') || 1;

    this.shadowRoot.innerHTML = `
      <!-- Styles and UI omitted here for brevity -->
    `;

    this.input = this.shadowRoot.querySelector('input');
    this.btnMinus = this.shadowRoot.getElementById('btn-minus');
    this.btnPlus = this.shadowRoot.getElementById('btn-plus');

    this.internals.setFormValue(this.value);
    this.updateButtonState();

    this.btnMinus.onclick = () => this.updateValue(-1);
    this.btnPlus.onclick = () => this.updateValue(1);
    this.input.onchange = (e) => { this.value = e.target.value; };
  }
}
```

---

## Registro del componente

Para usar el componente es necesario registrarlo en JavaScript.

```javascript
customElements.define("plus-quantity", PlusQuantity);
```

---

## Flujo del componente

```
HTML
 ↓
plus-quantity
 ↓
Web Component
 ↓
Shadow DOM
 ↓
Genera UI (label + botones + input)
 ↓
Sincroniza valor con formulario
```

---

## Ventajas

* Permite seleccionar cantidades de forma intuitiva
* Evita errores de entrada manual
* Control automático de límites
* Integración directa con formularios HTML
* Encapsulación completa con Shadow DOM
* Interfaz consistente en todo el ERP

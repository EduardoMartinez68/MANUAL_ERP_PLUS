# ConfirmButton

`ConfirmButton` es un **Web Component** que crea un **botón con confirmación previa** antes de ejecutar una acción.

Cuando el usuario hace clic en el botón, el componente muestra un **popup de confirmación** con un mensaje y dos opciones:

* **Aceptar**
* **Cancelar**

Solo si el usuario confirma la acción se ejecutará la función indicada.

Este componente es útil para acciones sensibles del ERP como:

* Eliminar registros
* Confirmar operaciones
* Ejecutar procesos críticos

---

## Uso básico

El programador solo necesita usar la etiqueta personalizada en HTML.

```html
<confirm-button onclick="deleteUser()">
Delete
</confirm-button>
```

Resultado:

```id="basic-confirm"
[ Delete ]
```

Al hacer clic aparecerá un **popup de confirmación** antes de ejecutar `deleteUser()`.

---

## Atributos

| Atributo  | Descripción                                        |
| --------- | -------------------------------------------------- |
| `onclick` | Nombre de la función que se ejecutará al confirmar |
| `title`   | Título del popup de confirmación                   |
| `message` | Mensaje que se mostrará en el popup                |

---

## Ejemplo completo

```html
<confirm-button
    title="Confirm delete"
    message="Do you really want to delete this record?"
    onclick="deleteRecord()">

    Delete Record

</confirm-button>
```

---

## Popup generado

Cuando el usuario hace clic en el botón se genera dinámicamente un popup similar a:

```html
<div class="confirm-popup-overlay">

  <div class="confirm-popup">

    <div class="icon-circle">?</div>

    <p>Do you really want to delete this record?</p>

    <div class="popup-buttons">
        <button class="popup-accept">Ok</button>
        <button class="popup-cancel">Cancel</button>
    </div>

  </div>

</div>
```

---

## Comportamiento

Cuando el componente se monta en el DOM:

1. Obtiene los atributos `title`, `message` y `onclick`.
2. Traduce el texto si existe en el diccionario `translateOld`.
3. Crea un botón con la clase `confirm-button`.
4. Cuando el usuario hace clic:

   * Se genera un popup de confirmación.
5. El popup muestra:

   * Icono
   * Mensaje
   * Botón **Aceptar**
   * Botón **Cancelar**.

### Acciones del popup

**Cancelar**

```text
Cierra el popup sin ejecutar ninguna acción.
```

**Aceptar**

```text
Cierra el popup y ejecuta la función indicada en el atributo onclick.
```

---

## Sistema de traducción

El componente soporta traducciones usando:

* `translateOld`
* `LANG`

Ejemplo:

```javascript
translateOld["confirm_delete"] = "Confirm delete";
LANG["message.cancel"] = "Cancel";
LANG["message.success"] = "Ok";
```

---

## Ejemplo de función ejecutada

```javascript
function deleteRecord() {
    console.log("Record deleted");
}
```

---

## Código fuente

```javascript
class ConfirmButton extends HTMLElement {
  constructor() {
    super();
  }

  connectedCallback() {

    const textTitle = this.getAttribute("title") || "";
    const title = translateOld[textTitle] || textTitle;

    const textMessage = this.getAttribute("message") || "Are you sure?";
    const message = translateOld[textMessage] || textMessage;

    const onclickAttr = this.getAttribute("onclick");
    const buttonText = this.textContent.trim();

    const button = document.createElement("button");
    button.textContent = buttonText;
    button.classList.add("confirm-button");

    button.addEventListener("click", () => {
      this.showPopup(title, message, onclickAttr);
    });

    this.replaceWith(button);
  }

  showPopup(title, message, onclickAttr) {

    const overlay = document.createElement("div");
    overlay.className = "confirm-popup-overlay";

    const popup = document.createElement("div");
    popup.className = "confirm-popup";

    const iconCircle = document.createElement("div");
    iconCircle.className = "icon-circle";
    iconCircle.textContent = "?";

    const titleElement = document.createElement("h4");
    titleElement.textContent = title;

    const msg = document.createElement("p");
    msg.textContent = message;

    const buttons = document.createElement("div");
    buttons.className = "popup-buttons";

    const acceptBtn = document.createElement("button");
    acceptBtn.textContent = LANG['message.success'] || "Ok";
    acceptBtn.className = "popup-accept";

    const cancelBtn = document.createElement("button");
    cancelBtn.textContent = LANG['message.cancel'] || "Cancel";
    cancelBtn.className = "popup-cancel";

    cancelBtn.onclick = () => overlay.remove();

    acceptBtn.onclick = () => {
      overlay.remove();

      const functionName = onclickAttr.replace(/\(\)/, "");

      if (onclickAttr && typeof window[functionName] === "function") {
        window[functionName]();
      } else {
        console.warn(`The function "${functionName}" was not found in window.`);
      }
    };

    buttons.appendChild(acceptBtn);
    buttons.appendChild(cancelBtn);

    popup.appendChild(iconCircle);
    popup.appendChild(msg);
    popup.appendChild(buttons);

    overlay.appendChild(popup);
    document.body.appendChild(overlay);
  }
}
```

---

## Registro del componente

Para usar el componente es necesario registrarlo en JavaScript.

```javascript
customElements.define("confirm-button", ConfirmButton);
```

---

## Flujo del componente

```text
HTML
 ↓
confirm-button
 ↓
Web Component
 ↓
Genera botón
 ↓
Usuario hace clic
 ↓
Se muestra popup de confirmación
 ↓
Aceptar → Ejecuta función
Cancelar → Cierra popup
```

---

## Ventajas

* Evita ejecuciones accidentales de acciones críticas
* Interfaz de confirmación consistente en todo el ERP
* Fácil de usar con una sola etiqueta HTML
* Soporte para internacionalización
* Permite ejecutar funciones dinámicamente
* Mejora la experiencia de usuario

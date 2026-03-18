# ConfirmDialog

`ConfirmDialog` es una **clase JavaScript** que permite mostrar un **popup de confirmación asíncrono** utilizando **Promises**.

A diferencia de `ConfirmButton`, este sistema permite solicitar confirmación **desde cualquier parte del código JavaScript**, esperando la respuesta del usuario antes de continuar la ejecución.

Este patrón es muy útil para:

* Confirmar eliminación de registros
* Confirmar acciones críticas
* Validar decisiones del usuario
* Controlar flujos de ejecución

---

## Uso básico

El método principal es `ConfirmDialog.show()`.

```javascript id="y2gn9l"
const confirmed = await ConfirmDialog.show(
  "Confirm action",
  "Are you sure?"
);
```

Si el usuario acepta:

```id="confirm-true"
true
```

Si el usuario cancela:

```id="confirm-false"
false
```

---

## Ejemplo completo

```javascript id="zn34ux"
async function deleteRecord() {

  const confirmed = await ConfirmDialog.show(
    "Delete record",
    "Do you really want to delete this record?"
  );

  if (confirmed) {
    console.log("Record deleted");
  } else {
    console.log("Operation cancelled");
  }

}
```

---

## Comportamiento

Cuando se llama `ConfirmDialog.show()`:

1. Se crea un **overlay oscuro** que bloquea la interfaz.
2. Se genera un **popup de confirmación**.
3. El popup incluye:

   * Icono
   * Título
   * Mensaje
   * Botón **Aceptar**
   * Botón **Cancelar**
4. La función devuelve una **Promise**.
5. Cuando el usuario interactúa:

   * **Aceptar → `resolve(true)`**
   * **Cancelar → `resolve(false)`**

---

## Popup generado

El sistema crea dinámicamente una estructura similar a:

```html id="8l8qmf"
<div class="confirm-popup-overlay">

  <div class="confirm-popup">

    <div class="icon-circle">?</div>

    <h4>Confirm action</h4>

    <p>Are you sure?</p>

    <div class="popup-buttons">
        <button class="popup-accept">Ok</button>
        <button class="popup-cancel">Cancel</button>
    </div>

  </div>

</div>
```

---

## Sistema de traducción

El componente utiliza dos sistemas de traducción:

### Diccionario `translateOld`

Permite traducir el título y el mensaje.

```javascript id="f7yptp"
translateOld["delete_record"] = "Delete record";
translateOld["confirm_delete"] = "Do you really want to delete this record?";
```

### Diccionario `LANG`

Se utiliza para traducir los botones.

```javascript id="v3k91p"
LANG["message.success"] = "Ok";
LANG["message.cancel"] = "Cancel";
```

---

## Control de capas (z-index)

El popup utiliza la variable global:

```javascript id="eq4l7f"
currentPopZIndex
```

Esto permite manejar múltiples popups o componentes modales sin conflictos de capas.

---

## Código fuente

```javascript id="3b0k8n"
class ConfirmDialog {
  static show(title, message) {
    return new Promise((resolve) => {

      const overlay = document.createElement("div");
      overlay.className = "confirm-popup-overlay";
      overlay.style.zIndex = currentPopZIndex;

      const popup = document.createElement("div");
      popup.className = "confirm-popup";
      popup.style.zIndex = currentPopZIndex;

      const iconCircle = document.createElement("div");
      iconCircle.className = "icon-circle";
      iconCircle.textContent = "?";

      const titleElement = document.createElement("h4");
      titleElement.textContent = translateOld[title] || title;

      const msg = document.createElement("p");
      msg.textContent = translateOld[message] || message;

      const buttons = document.createElement("div");
      buttons.className = "popup-buttons";

      const acceptBtn = document.createElement("button");
      acceptBtn.textContent = LANG['message.success'] || "Ok";
      acceptBtn.className = "popup-accept";

      const cancelBtn = document.createElement("button");
      cancelBtn.textContent = LANG['message.cancel'] || "Cancel";
      cancelBtn.className = "popup-cancel";

      cancelBtn.onclick = () => {
        overlay.remove();
        resolve(false);
      };

      acceptBtn.onclick = () => {
        overlay.remove();
        resolve(true);
      };

      buttons.appendChild(acceptBtn);
      buttons.appendChild(cancelBtn);

      popup.appendChild(iconCircle);
      popup.appendChild(titleElement);
      popup.appendChild(msg);
      popup.appendChild(buttons);

      overlay.appendChild(popup);
      document.body.appendChild(overlay);

    });
  }
}
```

---

## Flujo de ejecución

```text id="6v1uod"
JavaScript
 ↓
ConfirmDialog.show()
 ↓
Se crea popup dinámico
 ↓
Usuario interactúa
 ↓
Aceptar → Promise resolve(true)
Cancelar → Promise resolve(false)
 ↓
El código continúa ejecutándose
```

---

## Ventajas

* Permite confirmaciones desde cualquier parte del código
* Controla flujos de ejecución usando `async/await`
* Interfaz consistente para confirmaciones
* Soporte para internacionalización
* Manejo de capas mediante `z-index`
* Fácil integración con cualquier módulo del ERP

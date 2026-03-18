# KeyBind

`KeyBind` es un **Web Component** que permite definir **atajos de teclado (keyboard shortcuts)** directamente desde HTML.

Este componente escucha combinaciones de teclas (por ejemplo `Ctrl + S`, `Shift + V`, etc.) y ejecuta automáticamente una función definida en `window`.

Esto permite **declarar atajos de teclado sin escribir código JavaScript adicional**, solo usando etiquetas HTML.

---

# Uso básico

El programador puede definir un atajo de teclado de esta forma:

```html
<key-bind combo="shift+v" action="openVentas"></key-bind>
```

Cuando el usuario presione:

```
Shift + V
```

se ejecutará:

```javascript
window.openVentas()
```

---

# Atributos

| Atributo | Tipo   | Descripción                                  |
| -------- | ------ | -------------------------------------------- |
| `combo`  | string | Combinación de teclas                        |
| `action` | string | Nombre de la función global que se ejecutará |

---

# Ejemplo práctico

### HTML

```html
<key-bind combo="ctrl+s" action="saveInvoice"></key-bind>
<key-bind combo="shift+v" action="openVentas"></key-bind>
<key-bind combo="alt+p" action="openProducts"></key-bind>
```

### JavaScript

```javascript
function saveInvoice() {
  console.log("Factura guardada");
}

function openVentas() {
  console.log("Abrir módulo de ventas");
}

function openProducts() {
  console.log("Abrir productos");
}
```

---

# Flujo de funcionamiento

Cuando el componente se carga en el DOM:

```
<key-bind>
      ↓
Lee atributos combo y action
      ↓
Divide la combinación de teclas
      ↓
Escucha eventos keydown y keyup
      ↓
Detecta si todas las teclas están presionadas
      ↓
Ejecuta window[action]()
```

---

# Procesamiento del combo de teclas

La combinación se obtiene desde el atributo:

```javascript
const combo = (this.getAttribute("combo") || "")
```

Luego se normaliza:

```javascript
.toLowerCase().replace(/\s+/g, '')
```

Esto permite aceptar formatos como:

```
Shift + V
shift+v
SHIFT+V
```

Todos se convierten en:

```
shift+v
```

---

# Separación de teclas

La combinación se divide usando `+`:

```javascript
const keys = combo.split('+');
```

Ejemplo:

```
shift+v
```

se convierte en:

```
["shift", "v"]
```

---

# Detección de teclas presionadas

Se utiliza un `Set` para almacenar las teclas actualmente presionadas.

```javascript
const pressedKeys = new Set();
```

Cuando el usuario presiona una tecla:

```javascript
pressedKeys.add(e.key.toLowerCase());
```

Cuando la suelta:

```javascript
pressedKeys.delete(e.key.toLowerCase());
```

---

# Verificación de la combinación

En cada `keydown` se verifica si **todas las teclas del combo están presionadas**.

```javascript
const allKeysPressed = keys.every(k => pressedKeys.has(k.toLowerCase()));
```

Si todas coinciden:

```
Shift + V
```

se ejecuta la acción.

---

# Ejecución de la acción

La función se busca en el objeto global `window`.

```javascript
window[actionName]()
```

Ejemplo:

```
action="openVentas"
```

ejecuta:

```javascript
window.openVentas()
```

Si la función no existe, se muestra una advertencia en consola.

```javascript
console.warn("Función no encontrada")
```

---

# Prevención de repeticiones

Después de ejecutar la acción, se limpia el `Set`.

```javascript
pressedKeys.clear();
```

Esto evita que el evento se dispare múltiples veces si el usuario mantiene presionadas las teclas.

---

# Ocultar el componente

El elemento `<key-bind>` no tiene función visual, por lo que se oculta automáticamente.

```javascript
this.style.display = "none";
```

Esto permite mantener la estructura declarativa en HTML sin afectar la interfaz.

---

# Ejemplo completo

```html
<key-bind combo="ctrl+s" action="saveInvoice"></key-bind>

<script>
function saveInvoice() {
  alert("Factura guardada");
}
</script>
```

---

# Código fuente

```javascript
class KeyBind extends HTMLElement {

  constructor() {
    super();
  }

  connectedCallback() {

    const combo = (this.getAttribute("combo") || "")
      .toLowerCase()
      .replace(/\s+/g, '');

    const actionName = this.getAttribute("action");

    if (!combo || !actionName) return;

    const keys = combo.split('+');
    const pressedKeys = new Set();

    const onKeyDown = (e) => {

      pressedKeys.add(e.key.toLowerCase());

      const allKeysPressed = keys.every(k =>
        pressedKeys.has(k.toLowerCase())
      );

      if (allKeysPressed) {

        if (typeof window[actionName] === 'function') {
          window[actionName]();
        } else {
          console.warn(`Función '${actionName}' no encontrada en window`);
        }

        pressedKeys.clear();
      }
    };

    const onKeyUp = (e) => {
      pressedKeys.delete(e.key.toLowerCase());
    };

    window.addEventListener('keydown', onKeyDown);
    window.addEventListener('keyup', onKeyUp);

    this.style.display = "none";
  }
}
```

---

# Ventajas

* Permite definir **atajos de teclado directamente en HTML**
* Reduce código JavaScript repetitivo
* Facilita agregar nuevos shortcuts
* Mantiene la lógica de UI declarativa
* Soporta múltiples combinaciones de teclas
* Integración sencilla con funciones globales del sistema

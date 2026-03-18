# CreateHelp

`CreateHelp` es una **clase de JavaScript** diseñada para **crear y registrar pasos de ayuda** que luego serán utilizados por el componente `plus-help`.

Esta clase permite **construir tutoriales o guías paso a paso de forma programática**, guardando la información en la variable global:

```javascript
window.PLUS_HELP_DATA
```

En resumen:

**`CreateHelp` = herramienta para crear contenido de ayuda que usará el componente `plus-help`.**

---

# Propósito de la clase

El objetivo de `CreateHelp` es facilitar la creación de guías interactivas para el usuario.

Estas guías pueden incluir:

* títulos
* texto explicativo
* imágenes
* videos

Luego el componente `plus-help` mostrará esos pasos dentro de su interfaz.

---

# Código base

```javascript
class CreateHelp {
```

Esto define una **clase de JavaScript** que se utilizará para construir pasos de ayuda.

---

# constructor()

```javascript
constructor(key) {
  this.key = key;
  this.title = '';
  this.message = '';
  this.image = '';
  this.video = '';
}
```

El constructor inicializa la instancia de la clase.

---

# key

```javascript
this.key = key;
```

`key` es **la clave que identifica el conjunto de ayuda**.

Ejemplo:

```javascript
new CreateHelp("invoice-help")
```

Esto creará o utilizará el objeto:

```javascript
window.PLUS_HELP_DATA["invoice-help"]
```

---

# Variables internas

La clase guarda temporalmente la información del paso actual.

```javascript
this.title
this.message
this.image
this.video
```

Estas propiedades representan:

```text
title   → título del paso
message → texto explicativo
image   → imagen opcional
video   → video opcional
```

---

# toStepObject()

```javascript
toStepObject()
```

Este método **convierte los datos actuales en un objeto de paso de ayuda**.

Código:

```javascript
toStepObject() {
  const step = {
    title: this.title,
    text: this.message,
  };

  if (this.image) step.image = this.image;
  if (this.video) step.video = this.video;

  return step;
}
```

---

# Qué hace este método

Crea un objeto con la estructura que usa `plus-help`.

Ejemplo de resultado:

```javascript
{
  title: "Create invoice",
  text: "Click the create button",
  image: "/img/invoice.png",
  video: "dQw4w9WgXcQ"
}
```

Las propiedades `image` y `video` solo se agregan si tienen valor.

---

# add_step()

```javascript
add_step({ title = '', message = '', image = '', video = '' })
```

Este método **agrega un nuevo paso de ayuda**.

---

# Parámetros

El método recibe un objeto con las propiedades del paso.

```javascript
{
 title
 message
 image
 video
}
```

Ejemplo:

```javascript
help.add_step({
  title: "Create invoice",
  message: "Click the create button"
});
```

---

# Crear estructura de ayuda

Primero se verifica si ya existe la clave de ayuda.

```javascript
if (!window.PLUS_HELP_DATA[this.key]) {
  window.PLUS_HELP_DATA[this.key] = { steps: [] };
}
```

Si no existe, se crea automáticamente.

Resultado:

```javascript
window.PLUS_HELP_DATA = {
  "invoice-help": {
    steps: []
  }
}
```

---

# Guardar información del paso

Luego se guardan los valores en la instancia.

```javascript
this.title = title;
this.message = message;
this.image = image;
this.video = video;
```

Esto permite que el método `toStepObject()` genere el objeto correctamente.

---

# Agregar paso a la lista

Finalmente el paso se agrega al arreglo de pasos.

```javascript
window.PLUS_HELP_DATA[this.key].steps.push(this.toStepObject());
```

Resultado:

```javascript
window.PLUS_HELP_DATA["invoice-help"].steps.push({
  title: "...",
  text: "..."
});
```

---

# save_help_data()

```javascript
save_help_data()
```

Este método **guarda el paso actual sin recibir parámetros**.

Código:

```javascript
save_help_data() {
  if (!window.PLUS_HELP_DATA[this.key]) {
    window.PLUS_HELP_DATA[this.key] = { steps: [] };
  }

  window.PLUS_HELP_DATA[this.key].steps.push(this.toStepObject());
}
```

---

# Cuándo usar save_help_data()

Este método se usa cuando **los datos ya fueron asignados previamente**.

Ejemplo:

```javascript
const help = new CreateHelp("invoice-help");

help.title = "Create invoice";
help.message = "Click the create button";

help.save_help_data();
```

---

# Estructura final de datos

Después de agregar varios pasos, la estructura queda así:

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

Esta información será utilizada por el componente `plus-help`.

---

# Ejemplo completo

```javascript
window.PLUS_HELP_DATA = {};

const help = new CreateHelp("invoice-help");

help.add_step({
  title: "Create invoice",
  message: "Click the create button"
});

help.add_step({
  title: "Add products",
  message: "Select products from the list",
  image: "/img/products.png"
});

help.add_step({
  title: "Watch tutorial",
  message: "See how invoices work",
  video: "dQw4w9WgXcQ"
});
```

---

# Uso con plus-help

HTML:

```html
<plus-help data="invoice-help"></plus-help>
```

El componente cargará automáticamente los pasos definidos.

---

# Flujo de funcionamiento

```text
CreateHelp
     ↓
crear pasos de ayuda
     ↓
guardar en window.PLUS_HELP_DATA
     ↓
plus-help lee esos datos
     ↓
mostrar tutorial paso a paso
```

---

# Ventajas de esta clase

Usar `CreateHelp` tiene varias ventajas:

* facilita crear guías paso a paso
* organiza la información de ayuda
* compatible con `plus-help`
* permite agregar imágenes y videos
* simplifica la documentación interactiva

---

# Conclusión

`CreateHelp` es una **clase utilitaria para crear contenido de ayuda estructurado** que será utilizado por el componente `plus-help`.

Permite construir tutoriales interactivos dentro de una aplicación web de forma simple, flexible y reutilizable.

# set_value_plus_select_multiple

`set_value_plus_select_multiple` es una **función auxiliar (helper)** que permite **establecer múltiples valores en un componente `plus-multi-select` usando su `id`**.

Esta función facilita trabajar con el componente cuando se cargan datos desde:

* APIs
* formularios de edición
* respuestas del servidor
* bases de datos

En resumen:

**`set_value_plus_select_multiple` = forma sencilla de asignar múltiples valores a un `plus-multi-select`.**

---

# Código fuente

```javascript
function set_value_plus_select_multiple(componentId, values) {
  const multiSelect = document.getElementById(componentId);
  if (!multiSelect || typeof multiSelect.setValues !== "function") return;

  // Arrays para IDs y textos
  let ids = [];
  let texts = null;

  if (!Array.isArray(values)) return;

  // Detectar si es array de objetos [{id,name}]
  if (values.length > 0 && typeof values[0] === "object") {
    ids = values.map(item => item.id);
    texts = values.map(item => item.name ?? String(item.id));
  } else {
    // Array simple de IDs
    ids = values.map(val => String(val)); // forzar a string
  }

  // Llamar al setValues del componente
  multiSelect.setValues(ids, texts);
}
```

---

# Qué hace esta función

La función permite **establecer múltiples selecciones en un `plus-multi-select`** de forma flexible.

Puede recibir datos en dos formatos:

```
1. Array de IDs
2. Array de objetos con id y nombre
```

Luego convierte esos datos al formato que el componente necesita y ejecuta:

```
setValues(ids, texts)
```

---

# Parámetros

La función recibe **dos parámetros**.

---

# componentId

Es el `id` del componente `plus-multi-select`.

```javascript
componentId
```

Ejemplo:

```html
<plus-multi-select id="users"></plus-multi-select>
```

Uso:

```
users
```

---

# values

Es el conjunto de valores que se seleccionarán.

```javascript
values
```

Debe ser un **array**.

Puede tener dos formatos diferentes.

---

# Paso 1 — Buscar el componente

Primero se busca el elemento en el DOM.

```javascript
const multiSelect = document.getElementById(componentId);
```

Si el componente no existe o no tiene el método `setValues`, la función termina.

```javascript
if (!multiSelect || typeof multiSelect.setValues !== "function") return;
```

Esto evita errores de JavaScript.

---

# Paso 2 — Validar el array

La función verifica que el parámetro `values` sea un array.

```javascript
if (!Array.isArray(values)) return;
```

Ejemplo válido:

```
[1,2,3]
```

Ejemplo inválido:

```
"1,2,3"
```

---

# Paso 3 — Detectar el tipo de datos

La función detecta automáticamente el formato de los datos.

```javascript
if (values.length > 0 && typeof values[0] === "object")
```

Esto permite soportar:

```
[{id:1,name:"Edward"}]
```

---

# Caso 1 — Array de objetos

Si el array contiene objetos:

```javascript
[
 {id:1,name:"Edward"},
 {id:2,name:"Maria"}
]
```

La función separa:

```
IDs
Textos
```

Código:

```javascript
ids = values.map(item => item.id);
texts = values.map(item => item.name ?? String(item.id));
```

Resultado:

```
ids   = [1,2]
texts = ["Edward","Maria"]
```

Esto permite mostrar los nombres sin tener que cargarlos desde el servidor.

---

# Caso 2 — Array de IDs

Si el array contiene valores simples:

```javascript
[1,2,3]
```

Entonces solo se usan los IDs.

Código:

```javascript
ids = values.map(val => String(val));
```

Resultado:

```
ids = ["1","2","3"]
```

Los valores se convierten a **string** para evitar problemas de comparación.

---

# Paso 4 — Asignar valores al componente

Finalmente se llama al método del componente:

```javascript
multiSelect.setValues(ids, texts);
```

Este método actualiza:

* las opciones seleccionadas
* el texto visible
* el input oculto del formulario

---

# Ejemplo básico

HTML:

```html
<plus-multi-select
id="users"
name="users">
</plus-multi-select>
```

JavaScript:

```javascript
set_value_plus_select_multiple("users", [1,3,5]);
```

Resultado:

```
Usuarios seleccionados:
1
3
5
```

---

# Ejemplo con objetos

```javascript
set_value_plus_select_multiple("users", [
  {id:1,name:"Edward"},
  {id:2,name:"Maria"}
]);
```

Resultado visual:

```
Edward
Maria
```

Esto evita tener que buscar los nombres en el servidor.

---

# Ejemplo con datos del servidor

```javascript
fetch("/api/invoice/15")
  .then(r => r.json())
  .then(data => {

    set_value_plus_select_multiple(
      "products",
      data.products
    );

  });
```

Respuesta del servidor:

```json
{
  "products":[
    {"id":1,"name":"Laptop"},
    {"id":3,"name":"Mouse"}
  ]
}
```

---

# Ejemplo en formularios de edición

```javascript
function loadProject(project) {

  set_value_plus_select_multiple(
    "members",
    project.members
  );

}
```

Esto carga automáticamente los usuarios seleccionados.

---

# Ventajas de esta función

Esta función tiene varias ventajas:

* soporta múltiples formatos de datos
* evita errores si el componente no existe
* simplifica el código
* compatible con APIs
* útil para formularios de edición
* reduce código repetido

---

# Comparación con acceso directo

Sin helper:

```javascript
const select = document.getElementById("users");

if (select && typeof select.setValues === "function") {
  select.setValues([1,2],["Edward","Maria"]);
}
```

Con helper:

```javascript
set_value_plus_select_multiple("users", [
 {id:1,name:"Edward"},
 {id:2,name:"Maria"}
]);
```

Mucho más limpio.

---

# Flujo de funcionamiento

```
set_value_plus_select_multiple()
            ↓
buscar componente
            ↓
validar array
            ↓
detectar formato de datos
            ↓
convertir a ids y textos
            ↓
ejecutar setValues()
```

---

# Cuándo usar esta función

Es especialmente útil cuando:

* se cargan formularios de edición
* se reciben datos desde una API
* se necesita seleccionar múltiples registros
* se trabaja con `plus-multi-select`

---

# Conclusión

`set_value_plus_select_multiple` es un **helper que simplifica el manejo de selecciones múltiples** en el componente `plus-multi-select`.

Permite trabajar con datos del servidor **sin preocuparse por el formato exacto**, haciendo el código más limpio, flexible y reutilizable.

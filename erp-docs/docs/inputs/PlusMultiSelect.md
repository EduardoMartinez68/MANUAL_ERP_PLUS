# PlusMultiSelect

`PlusMultiSelect` es un **Web Component que reemplaza un `<select multiple>` tradicional** por un **selector avanzado que permite elegir múltiples elementos**, con buscador, carga dinámica desde servidor y acciones como crear, editar o eliminar registros.

Este componente está pensado para **interfaces modernas de aplicaciones empresariales** como ERP, CRM o dashboards.

En resumen:

**`PlusMultiSelect` = selector múltiple inteligente.**

---

# Qué hace este componente

Convierte algo como:

```html
<plus-multi-select name="tags">
  <option value="1">Finance</option>
  <option value="2">Sales</option>
  <option value="3">Marketing</option>
</plus-multi-select>
```

en un **selector con popup donde el usuario puede elegir varias opciones**.

Visualmente funciona así:

```
[ Select tags > ]

click →

┌─────────────────────────┐
│ 🔎 search...            │
│                         │
│ ☑ Finance               │
│ ☐ Sales                 │
│ ☑ Marketing             │
│ ☐ Development           │
└─────────────────────────┘
```

El usuario puede **seleccionar múltiples opciones**.

---

# Características principales

`PlusMultiSelect` incluye funcionalidades avanzadas.

---

## 1. Selección múltiple

Permite seleccionar varias opciones al mismo tiempo.

Ejemplo visual:

```
☑ Finance
☑ Marketing
☐ Sales
```

Internamente el componente guarda los valores seleccionados como un **array de IDs**.

Ejemplo enviado al formulario:

```
[1,3]
```

---

# 2. Buscador integrado

Incluye un campo para buscar entre las opciones.

```
🔎 search...
```

Esto permite encontrar elementos rápidamente cuando existen muchos registros.

---

# 3. Carga dinámica desde servidor

El componente puede obtener opciones desde una API.

Ejemplo:

```
/api/tags?query=fin
```

Esto permite manejar **miles de registros sin cargar todo en el navegador**.

---

# 4. Mostrar imágenes o avatares

Las opciones pueden incluir una imagen.

Ejemplo visual:

```
[👤] Edward
[👤] Maria
```

Esto se usa comúnmente en selects de:

* usuarios
* clientes
* empleados

---

# 5. Mostrar colores

Las opciones pueden mostrar un indicador de color.

Ejemplo:

```
■ Urgent
■ Medium
■ Low
```

Esto es útil para selects como:

* prioridades
* estados
* categorías

---

# 6. Crear nuevos registros

Si el componente tiene el atributo `add`, aparecerá un botón:

```
[ + ]
```

Este botón ejecuta una función que el programador define.

---

# 7. Editar registros

Cada opción puede mostrar un botón de edición.

```
✏
```

Esto permite abrir un formulario para modificar el registro.

---

# 8. Eliminar registros

Las opciones también pueden incluir un botón para eliminar.

```
🗑
```

Antes de eliminar normalmente se muestra una confirmación.

---

# 9. Compatible con formularios

El componente crea automáticamente un **input oculto**:

```
<input type="hidden">
```

Este input guarda los IDs seleccionados en formato JSON.

Ejemplo enviado al formulario:

```
tags = [1,4,8]
```

---

# Parámetros (atributos)

---

# name

Define el nombre del campo que se enviará al formulario.

```
<plus-multi-select name="tags">
```

En el servidor se recibirá:

```
tags=[1,3,5]
```

---

# label

Define el texto visible del campo.

```
<plus-multi-select label="Tags">
```

---

# t

Permite usar una clave de traducción.

```
<plus-multi-select t="form.tags">
```

El sistema usa una función global para traducir el texto.

---

# value

Permite definir valores seleccionados por defecto.

```
<plus-multi-select
name="tags"
value="[1,3]">
```

---

# message

Muestra un mensaje de ayuda en el label.

```
<plus-multi-select
label="Tags"
message="Select the tags for this record">
```

---

# requerid

Hace que el campo sea obligatorio en el formulario.

```
<plus-multi-select
name="tags"
requerid>
```

---

# method

Define el método HTTP usado para consultar el servidor.

Valores posibles:

```
GET
POST
```

Ejemplo:

```
<plus-multi-select method="POST">
```

---

# link

URL usada para obtener opciones desde el servidor.

```
<plus-multi-select
name="customer"
link="/api/customers">
```

El servidor debe devolver una respuesta similar a:

```
{
  "success": true,
  "answer": [
    { "id": 1, "text": "Edward" },
    { "id": 2, "text": "Maria" }
  ]
}
```

---

# add

Permite mostrar un botón para crear nuevos registros.

```
<plus-multi-select add="createCustomer">
```

Cuando el usuario hace clic se ejecuta:

```
createCustomer()
```

---

# edit_data

Permite editar registros existentes.

```
<plus-multi-select edit_data="editCustomer()">
```

Cuando el usuario presiona editar se ejecuta:

```
editCustomer(id)
```

---

# delete_data

Permite eliminar registros.

```
<plus-multi-select delete_data="deleteCustomer()">
```

Se ejecutará la función pasando el ID del registro.

---

# btn_delete_title

Permite definir el título del mensaje de confirmación al eliminar.

```
<plus-multi-select
delete_data="deleteCustomer()"
btn_delete_title="Delete record">
```

---

# btn_delete_text

Permite definir el texto del mensaje de confirmación.

```
<plus-multi-select
delete_data="deleteCustomer()"
btn_delete_text="Are you sure you want to delete this item?">
```

---

# Ejemplo simple

```
<plus-multi-select name="categories" label="Categories">

  <option value="1">Food</option>
  <option value="2">Drinks</option>
  <option value="3">Desserts</option>

</plus-multi-select>
```

Resultado visual:

```
Categories
[ Select categories > ]
```

Al seleccionar varios:

```
3 seleccionados
```

---

# Ejemplo con servidor

```
<plus-multi-select
name="customers"
label="Customers"
link="/api/customers">
</plus-multi-select>
```

Cuando el usuario abre el select se consulta:

```
/api/customers
```

---

# Ejemplo con buscador

Si el usuario escribe:

```
mar
```

Se consulta al servidor:

```
/api/customers?query=mar
```

---

# Ejemplo completo avanzado

```
<plus-multi-select
name="users"
label="Users"
link="/api/users"
add="createUser"
edit_data="editUser()"
delete_data="deleteUser()">
</plus-multi-select>
```

Este selector permitirá:

```
🔎 Buscar usuarios
➕ Crear usuario
✏ Editar usuario
🗑 Eliminar usuario
☑ Seleccionar múltiples usuarios
```

---

# Funciones públicas

El componente puede controlarse desde JavaScript.

---

# setValues()

Permite establecer varios valores seleccionados.

```
select.setValues([1,3,5]);
```

También se puede pasar el texto de cada valor.

```
select.setValues(
  [1,3],
  ["Edward", "Maria"]
);
```

---

# getValue()

Obtiene el valor actual del selector.

```
const values = select.getValue();
```

Ejemplo resultado:

```
[1,3,5]
```

---

# reset()

Reinicia el componente.

```
select.reset();
```

Esto:

* limpia las selecciones
* borra el input oculto
* restaura el texto original

---

# Flujo de funcionamiento

```
plus-multi-select
      ↓
usuario hace click
      ↓
se abre popup
      ↓
usuario busca
      ↓
opciones se filtran
      ↓
usuario selecciona múltiples
      ↓
se guardan IDs en hidden input
      ↓
se dispara evento change
```

---

# Ventajas

Comparado con `<select multiple>` estándar:

* interfaz moderna
* buscador integrado
* carga desde servidor
* soporte de imágenes
* soporte de colores
* creación de registros
* edición de registros
* eliminación de registros
* selección múltiple intuitiva
* compatible con formularios
* ideal para aplicaciones empresariales

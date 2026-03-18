# PlusSelect

`PlusSelect` es un **Web Component avanzado que reemplaza el `<select>` tradicional de HTML** por un **selector moderno con buscador, carga dinámica desde servidor, acciones de editar/eliminar y soporte de traducciones**.

Este componente está diseñado para sistemas complejos como **ERP, CRM o dashboards**, donde los selects necesitan:

* Buscar registros
* Cargar datos desde el servidor
* Mostrar imágenes o colores
* Permitir crear nuevos registros
* Permitir editar o eliminar opciones
* Soportar traducciones

En resumen:

**`PlusSelect` = Select inteligente para aplicaciones empresariales.**

---

# Qué hace este componente

El componente transforma algo como esto:

```html
<plus-select name="customer">
  <option value="1">John</option>
  <option value="2">Mary</option>
</plus-select>
```

en un **selector interactivo con popup**, buscador y opciones dinámicas.

Visualmente funciona así:

```
[ Select customer  > ]

click →

┌─────────────────────┐
│ 🔎 search...        │
│                     │
│ John                │
│ Mary                │
│ Pedro               │
└─────────────────────┘
```

---

# Características principales

`PlusSelect` incluye muchas funcionalidades avanzadas:

### 1. Buscador integrado

Permite buscar entre las opciones.

```
🔎 search...
```

---

### 2. Carga dinámica desde servidor

Puede obtener opciones desde una API.

Ejemplo:

```
/api/customers?query=ed
```

---

### 3. Soporte de traducciones

Todos los textos pueden pasar por:

```
translate_text()
```

---

### 4. Mostrar imágenes o avatares

Opciones pueden incluir:

```
photo
avatar
```

Ejemplo visual:

```
[👤] Edward
[👤] Maria
```

---

### 5. Mostrar colores

Se puede mostrar un indicador de color.

Ejemplo:

```
■ Urgent
■ Normal
■ Low priority
```

---

### 6. Crear nuevos registros

Si el componente tiene el atributo `add`, aparece un botón:

```
[ + ]
```

---

### 7. Editar registros

Cada opción puede tener botón:

```
✏
```

---

### 8. Eliminar registros

Cada opción puede tener botón:

```
🗑
```

---

### 9. Hidden input automático

Internamente el componente crea:

```
<input type="hidden">
```

Este es el valor real que se envía en formularios.

---

# Parámetros (atributos)

## name

Nombre del input que se enviará en el formulario.

```html
<plus-select name="customer">
```

---

## label o t

Texto del label.

```html
<plus-select label="Customer">
```

o

```html
<plus-select t="form.customer">
```

---

## value

Valor por defecto del select.

```html
<plus-select name="customer" value="5">
```

---

## method

Método HTTP usado para consultar el servidor.

Valores posibles:

```
GET
POST
```

Ejemplo:

```html
<plus-select method="POST">
```

---

## link

URL para obtener opciones desde el servidor.

```html
<plus-select
name="customer"
link="/api/customers">
```

El servidor debe devolver algo como:

```json
{
 "success": true,
 "answer": [
   { "id": 1, "text": "Edward" },
   { "id": 2, "text": "Maria" }
 ]
}
```

---

## add

Permite agregar un nuevo registro.

Debe contener el nombre de una función.

```html
<plus-select add="createCustomer">
```

---

## edit_data

Permite editar registros.

```html
<plus-select edit_data="editCustomer()">
```

---

## delete_data

Permite eliminar registros.

```html
<plus-select delete_data="deleteCustomer()">
```

---

## message

Permite mostrar un mensaje de ayuda en el label.

```html
<plus-select
label="Customer"
message="Select the customer for this invoice">
```

---

## requerid

Hace el campo obligatorio.

```html
<plus-select name="customer" requerid>
```

---

# Ejemplo simple

```html
<plus-select name="category" label="Category">

  <option value="1">Food</option>
  <option value="2">Drinks</option>
  <option value="3">Desserts</option>

</plus-select>
```

Resultado:

```
Category
[ Select category > ]
```

---

# Ejemplo con servidor

```html
<plus-select
name="customer"
label="Customer"
link="/api/customers">
</plus-select>
```

Cuando el usuario abre el select se consulta:

```
/api/customers
```

---

# Ejemplo con buscador

El usuario escribe:

```
ed
```

La consulta será:

```
/api/customers?query=ed
```

---

# Ejemplo completo avanzado

```html
<plus-select
name="customer"
label="Customer"
link="/api/customers"
add="createCustomer"
edit_data="editCustomer()"
delete_data="deleteCustomer()">
</plus-select>
```

Este select permitirá:

```
🔎 Buscar clientes
➕ Crear cliente
✏ Editar cliente
🗑 Eliminar cliente
```

---

# Funciones públicas

El componente también puede controlarse desde JavaScript.

---

## setValue()

Permite establecer el valor del select.

```javascript
document.querySelector("plus-select")
        .setValue(5);
```

También puede usarse con objeto:

```javascript
select.setValue({
  id: 5,
  name: "Edward"
});
```

---

## getValue()

Obtiene el valor actual.

```javascript
const value = select.getValue();
```

---

## reset()

Reinicia el select.

```javascript
select.reset();
```

---

# Ejemplo en formulario

```html
<form>

  <plus-select
  name="customer"
  label="Customer"
  link="/api/customers">
  </plus-select>

  <button type="submit">
    Save
  </button>

</form>
```

El formulario enviará:

```
customer=5
```

---

# Flujo de funcionamiento

```
plus-select
      ↓
usuario hace click
      ↓
se abre popup
      ↓
usuario busca
      ↓
opciones se filtran
      ↓
usuario selecciona
      ↓
se actualiza hidden input
      ↓
se dispara evento change
```

---

# Ventajas

`PlusSelect` tiene muchas ventajas sobre `<select>` normal:

* Buscador integrado
* Carga dinámica desde API
* Soporte para imágenes
* Soporte para colores
* Permite editar registros
* Permite eliminar registros
* Permite crear registros
* Traducciones automáticas
* Compatible con formularios
* Interfaz moderna
* Ideal para ERP o CRM

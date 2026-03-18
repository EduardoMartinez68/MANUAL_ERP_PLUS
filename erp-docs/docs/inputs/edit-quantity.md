# EditQuantity

`<edit-quantity>` es un componente de **PLUS UI** diseñado para permitir al usuario **editar cantidades de forma rápida mediante un popup interactivo**.

Este componente muestra inicialmente un **campo de solo lectura**, y cuando el usuario hace clic en él, se abre un **popup modal** donde puede modificar la cantidad usando:

- Botones de incremento `+`
- Botones de decremento `−`
- Entrada manual numérica

Este componente fue creado especialmente para operaciones comunes en sistemas ERP como:

- Edición de cantidades en ventas
- Modificación de productos en pedidos
- Ajustes de inventario
- Edición rápida de valores numéricos

El objetivo principal es **evitar errores de escritura y mejorar la experiencia del usuario** al modificar cantidades.

---

# Uso básico

```html
<edit-quantity name="quantity"></edit-quantity>
```

---

## Atributos

| Atributo | Descripción |
|--------|-------------|
| `title` | El titulo del pop |
| `name` | Identificador único del input |
| `type` | Saber si es 'float' o 'int' |
| `step` | Como avanzara el contador al ahcer click en los botones |
| `value` | El valor inicial al mostrar el pop si es que deberia tener uno |
---
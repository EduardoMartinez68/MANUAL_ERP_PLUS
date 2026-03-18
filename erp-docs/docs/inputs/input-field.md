# InputField

`<input-field>` es un componente de **PLUS UI** diseñado para crear campos de entrada estandarizados dentro del ERP.

Este componente simplifica la creación de formularios al encapsular:

- Etiquetas (`label`)
- Inputs HTML
- Validaciones básicas
- Traducción automática
- Mensajes de ayuda

El objetivo es permitir que los desarrolladores creen formularios **rápidos, consistentes y traducibles** utilizando una sola etiqueta personalizada.

---

# Uso básico

```html
<input-field
    label="Correo electrónico"
    name="email"
    type="email"
    placeholder="Ingrese su correo"
    required>
</input-field>
```

---

## Atributos

| Atributo | Tipo | Descripción |
|--------|------|-------------|
| `label` | string | Texto que se mostrará en la etiqueta del campo. También se utiliza como clave de traducción. |
| `name` | string | Nombre del campo que será enviado al servidor cuando el formulario se envíe. |
| `type` | string | Tipo de input HTML. Valores permitidos: `text`, `email`, `password`, `number`, `date`, `tel`. Si no se especifica, el valor por defecto es `text`. |
| `placeholder` | string | Texto que aparece dentro del input como guía para el usuario. Si no se define, se utilizará el texto del `label`. |
| `value` | string | Valor inicial del input cuando se renderiza el componente. |
| `required` | boolean | Indica si el campo es obligatorio. Activa validación visual automática. |
| `maxlength` | number | Número máximo de caracteres permitidos en el input. |
| `minlength` | number | Número mínimo de caracteres requeridos en el input. |
| `pattern` | string | Expresión regular que el valor del input debe cumplir para ser válido. |
| `readonly` | boolean | Hace que el input sea de solo lectura. El usuario no podrá modificar su valor. |
| `disabled` | boolean | Desactiva el input evitando cualquier interacción del usuario. |
| `help` | string | Texto de ayuda que se mostrará mediante el componente `info-label`. |
| `message-label` | string | Mensaje adicional que se mostrará arriba del input como etiqueta informativa. |
| `line` | boolean | Controla si el input muestra la línea inferior del estilo. Si es `false`, se elimina la línea. |
| `id` | string | Identificador único del input en el DOM. Si no se define, se genera automáticamente. |

---
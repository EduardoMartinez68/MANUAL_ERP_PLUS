# `format_date_to_text()`

La función `format_date_to_text()` convierte una fecha en formato ISO a un texto legible, permitiendo mostrarla en formato largo o corto según la necesidad.

---

## Sintaxis

```javascript
format_date_to_text(dateString, type = 1, locale = "es-ES")
```

---

## Parámetros

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `dateString` | String | Fecha en formato ISO. |
| `type` | Number | Tipo de formato. `1` = formato largo, cualquier otro valor = formato corto. |
| `locale` | String | Configuración regional utilizada para el formato de la fecha. El valor por defecto es `es-ES`. |

---

## Ejemplos

### Formato largo

```javascript
format_date_to_text("2025-09-05T15:00:00");
```

Resultado:

```text
5 de septiembre de 2025 a las 3:00 p. m.
```

---

### Formato corto

```javascript
format_date_to_text("2025-09-05T15:00:00", 2);
```

Resultado:

```text
05/09/2025 15:00
```

---

## Tipos disponibles

| Tipo | Resultado |
|------|-----------|
| `1` | Fecha larga con hora. |
| `2` (o cualquier otro valor) | Fecha y hora en formato `DD/MM/AAAA HH:MM`. |
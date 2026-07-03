# `format_date()`

La función `format_date()` convierte una fecha en formato `YYYY-MM-DD` a un formato corto y legible utilizando el idioma seleccionado para abreviar el nombre del mes.

---

## Sintaxis

```javascript
format_date(dateISO, lang = "en")
```

---

## Parámetros

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `dateISO` | String | Fecha en formato `YYYY-MM-DD`. |
| `lang` | String | Idioma utilizado para la abreviatura del mes. El valor por defecto es `en`. |

---

## Ejemplos

### Formato en inglés

```javascript
format_date("2025-09-05");
```

Resultado:

```text
05/Sep/2025
```

---

### Formato en español

```javascript
format_date("2025-09-05", "es");
```

Resultado:

```text
05/sep/2025
```

---

## Idiomas

La abreviatura del mes depende del idioma (`lang`) configurado en la aplicación. Si la fecha es inválida o no existe, la función devuelve el valor original recibido.
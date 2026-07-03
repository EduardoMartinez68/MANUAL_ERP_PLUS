# `format_currency()`

La función `format_currency()` permite convertir un número a un formato de moneda utilizando la configuración regional correspondiente al código de moneda indicado.

Es útil para mostrar importes de forma consistente dentro del ERP.

---

## Sintaxis

```javascript
format_currency(amount, currencyCode = "MXN", withCode = false, withSymbol = false)
```

---

## Parámetros

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `amount` | Number | Cantidad que se desea formatear. |
| `currencyCode` | String | Código ISO de la moneda (por ejemplo: `MXN`, `USD`, `EUR`). El valor por defecto es `MXN`. |
| `withCode` | Boolean | Agrega el código de la moneda al final del texto. |
| `withSymbol` | Boolean | Agrega el símbolo de la moneda al inicio del importe. |

---

## Ejemplos

### Formato básico

```javascript
format_currency(1500);
```

Resultado:

```text
1,500.00
```

---

### Mostrar símbolo

```javascript
format_currency(1500, "MXN", false, true);
```

Resultado:

```text
$ 1,500.00
```

---

### Mostrar símbolo y código

```javascript
format_currency(1500, "USD", true, true);
```

Resultado:

```text
$ 1,500.00 USD
```

---

## Monedas soportadas

- `MXN`
- `USD`
- `EUR`
- `GBP`
- `JPY`
- `BRL`
- `ARS`
- `CLP`
- `FRF`
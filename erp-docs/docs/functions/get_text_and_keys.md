# get_text_and_keys

`get_text_and_keys` es una **función utilitaria** utilizada por el sistema de **traducción del ERP** para extraer el **texto base** y las **claves dinámicas** que serán reemplazadas dentro de ese texto.

Permite aceptar diferentes tipos de estructuras como entrada y normalizarlas a un formato único:

```javascript
{ text, keys }
```

Esto facilita que el sistema de traducción procese siempre la información de forma consistente.

---

## Firma de la función

```javascript
function get_text_and_keys(struct)
```

### Parámetros

| Parámetro | Tipo                          | Descripción                                                             |
| --------- | ----------------------------- | ----------------------------------------------------------------------- |
| `struct`  | `string` | `array` | `object` | Estructura que contiene el texto a traducir y posibles claves dinámicas |

### Retorno

```javascript
{
  text: string,
  keys: object | array | null
}
```

| Propiedad | Descripción                                               |
| --------- | --------------------------------------------------------- |
| `text`    | Clave del texto a traducir                                |
| `keys`    | Valores dinámicos que serán reemplazados en la traducción |

---

## Formatos soportados

La función acepta **tres tipos de entrada**.

---

## 1. Texto simple

Si se envía un `string`, se interpreta como el texto a traducir.

```javascript
get_text_and_keys("app.label.name");
```

Resultado:

```javascript
{
  text: "app.label.name",
  keys: null
}
```

---

## 2. Array `[text, keys]`

Permite enviar el texto junto con valores dinámicos.

```javascript
get_text_and_keys([
  "message.user_welcome",
  { name: "Edward" }
]);
```

Resultado:

```javascript
{
  text: "message.user_welcome",
  keys: { name: "Edward" }
}
```

También puede usar un arreglo simple:

```javascript
get_text_and_keys([
  "message.items_count",
  ["Edward", 3]
]);
```

Resultado:

```javascript
{
  text: "message.items_count",
  keys: ["Edward", 3]
}
```

---

## 3. Objeto `{ text, keys }`

Permite una estructura más explícita.

```javascript
get_text_and_keys({
  text: "message.user_welcome",
  keys: { name: "Edward" }
});
```

Resultado:

```javascript
{
  text: "message.user_welcome",
  keys: { name: "Edward" }
}
```

---

## Ejemplo de uso con traducción

Supongamos que el diccionario tiene:

```javascript
LANG["message.user_welcome"] = "Welcome {name}";
```

Uso:

```javascript
const { text, keys } = get_text_and_keys([
  "message.user_welcome",
  { name: "Edward" }
]);
```

Después el sistema de traducción podría producir:

```text
Welcome Edward
```

---

## Comportamiento interno

La función realiza los siguientes pasos:

1. Inicializa dos variables:

   * `text`
   * `keys`
2. Detecta el tipo de la estructura recibida.
3. Extrae la información dependiendo del formato:

   * **string →** solo texto
   * **array →** `[text, keys]`
   * **object →** `{text, keys}`
4. Devuelve un objeto normalizado.

---

## Código fuente

```javascript
function get_text_and_keys(struct){

  let text = '';
  let keys = null;

  if (typeof struct === 'string') {
    text = struct;

  } else if (Array.isArray(struct)) {
    text = struct[0];
    keys = struct[1];

  } else if (typeof struct === 'object' && struct !== null) {
    text = struct.text;
    keys = struct.keys;
  }

  return { text, keys };
}
```

---

## Flujo de la función

```text
Entrada (struct)
       ↓
Detecta tipo de dato
       ↓
Extrae text y keys
       ↓
Normaliza estructura
       ↓
Devuelve { text, keys }
```

---

## Ventajas

* Permite usar **múltiples formatos de entrada**
* Simplifica el sistema de traducción
* Facilita el manejo de **variables dinámicas**
* Normaliza datos antes de procesarlos
* Mejora la flexibilidad del sistema de internacionalización

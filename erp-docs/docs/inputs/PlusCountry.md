# PlusCountry

`PlusCountry` es un **Web Component que crea automáticamente un selector de países** utilizando internamente el componente `plus-select`.

Su objetivo es **evitar que el programador tenga que escribir manualmente la lista de países**, generando automáticamente todas las opciones necesarias.

En otras palabras:

**`PlusCountry` = un `plus-select` preconfigurado para elegir países.**

---

# Qué hace este componente

Cuando usas:

```html
<plus-country name="country"></plus-country>
```

El componente **se transforma automáticamente en un `plus-select`** que contiene una lista de países con sus banderas.

Visualmente se verá algo así:

```
Select a country
[ 🇲🇽 México > ]

click →

🇺🇸 United States  
🇨🇦 Canada  
🇲🇽 México  
🇦🇷 Argentina  
🇪🇸 Spain  
```

---

# Cómo funciona internamente

El componente realiza estos pasos:

1. Define una **lista interna de países**.
2. Define un **mapa de emojis de banderas**.
3. Crea dinámicamente un componente `plus-select`.
4. Copia los atributos del `<plus-country>` al `<plus-select>`.
5. Genera todas las opciones (`<option>`).
6. Reemplaza el `<plus-country>` por el `plus-select`.

---

# Código fuente

```javascript
class PlusCountry extends HTMLElement {
  constructor() {
    super();
  }

  connectedCallback() {
```

`connectedCallback()` es un método de **Web Components** que se ejecuta cuando el elemento se inserta en el DOM.

---

## Lista de países

El componente define internamente un arreglo con países.

```javascript
const countries = [
  { code: "US", name: "United States" },
  { code: "CA", name: "Canada" },
  { code: "MX", name: "México" },
  { code: "AR", name: "Argentina" },
  { code: "BO", name: "Bolivia" },
  { code: "CL", name: "Chile" },
  { code: "CO", name: "Colombia" },
  { code: "CR", name: "Costa Rica" },
  { code: "CU", name: "Cuba" },
  { code: "DO", name: "Dominican Republic" },
  { code: "EC", name: "Ecuador" },
  { code: "SV", name: "El Salvador" },
  { code: "GT", name: "Guatemala" },
  { code: "HN", name: "Honduras" },
  { code: "NI", name: "Nicaragua" },
  { code: "PA", name: "Panama" },
  { code: "PY", name: "Paraguay" },
  { code: "PE", name: "Peru" },
  { code: "UY", name: "Uruguay" },
  { code: "VE", name: "Venezuela" },
  { code: "ES", name: "Spain" },
  { code: "PL", name: "Poland" }
];
```

Cada país tiene:

```
code → código ISO
name → nombre del país
```

Los códigos usan el estándar **ISO-3166 de dos letras**. ([Andy Croll][1])

---

# Mapa de banderas

El componente también define un objeto que relaciona el código del país con su emoji de bandera.

```javascript
const flagEmojiMap = {
  US: '🇺🇸',
  CA: '🇨🇦',
  MX: '🇲🇽',
  AR: '🇦🇷',
  BO: '🇧🇴',
  CL: '🇨🇱',
  CO: '🇨🇴',
  CR: '🇨🇷',
  CU: '🇨🇺',
  DO: '🇩🇴',
  EC: '🇪🇨',
  SV: '🇸🇻',
  GT: '🇬🇹',
  HN: '🇭🇳',
  NI: '🇳🇮',
  PA: '🇵🇦',
  PY: '🇵🇾',
  PE: '🇵🇪',
  UY: '🇺🇾',
  VE: '🇻🇪',
  ES: '🇪🇸',
  PL: '🇵🇱'
};
```

Las banderas emoji funcionan combinando **dos símbolos Unicode llamados "regional indicators"** que corresponden al código ISO del país. ([PyPI][2])

Por ejemplo:

```
US → 🇺 + 🇸 → 🇺🇸
```

---

# Creación del plus-select

El componente crea dinámicamente un `plus-select`.

```javascript
const plusSelect = document.createElement('plus-select');
```

Esto significa que **PlusCountry depende del componente PlusSelect** para funcionar.

---

# Valor por defecto

Se obtiene el valor inicial del atributo `value`.

```javascript
const value = this.getAttribute('value') || 'MX';
```

Si el programador no define un valor, el país por defecto será:

```
MX → México
```

---

# Copiar atributos

El componente copia los atributos del `<plus-country>` al `<plus-select>`.

```javascript
if (this.hasAttribute('t'))
  plusSelect.setAttribute('t', this.getAttribute('t'));

if (this.hasAttribute('name'))
  plusSelect.setAttribute('name', this.getAttribute('name'));

if (this.hasAttribute('requerid'))
  plusSelect.setAttribute('requerid', '');

if (this.hasAttribute('link'))
  plusSelect.setAttribute('link', this.getAttribute('link'));
```

Esto permite que `PlusCountry` soporte **los mismos parámetros que PlusSelect**.

---

# Crear las opciones

Luego el componente genera todas las opciones del selector.

```javascript
countries.forEach(country => {
  const option = document.createElement('option');

  option.value = country.code;
  option.setAttribute('t', country.name);

  option.textContent =
    `${flagEmojiMap[country.code] || ''} ${country.name}`;

  plusSelect.appendChild(option);
});
```

Cada opción contiene:

```
value → código del país
texto → bandera + nombre
```

Ejemplo generado:

```
<option value="US">🇺🇸 United States</option>
```

---

# Reemplazar el elemento

Finalmente el componente reemplaza el `<plus-country>` original por el `plus-select`.

```javascript
this.replaceWith(plusSelect);
```

Esto significa que en el DOM final **ya no existe `plus-country`**.

---

# Establecer el valor inicial

Después de crear el selector, se define el país seleccionado.

```javascript
setTimeout(() => {
  plusSelect.setValue(value);
}, 0);
```

Se usa `setTimeout` para asegurarse de que el componente esté completamente renderizado antes de asignar el valor.

---

# Parámetros del componente

`PlusCountry` acepta los siguientes atributos.

---

## name

Nombre del campo enviado al formulario.

```html
<plus-country name="country"></plus-country>
```

---

## value

País seleccionado por defecto.

```html
<plus-country
name="country"
value="US">
</plus-country>
```

---

## t

Clave de traducción del label.

```html
<plus-country
name="country"
t="form.country">
</plus-country>
```

---

## requerid

Hace el campo obligatorio.

```html
<plus-country
name="country"
requerid>
</plus-country>
```

---

## link

Permite obtener países desde una API en lugar de la lista interna.

```html
<plus-country
name="country"
link="/api/countries">
</plus-country>
```

---

# Ejemplo simple

```html
<plus-country name="country"></plus-country>
```

Resultado:

```
Select a country
[ 🇲🇽 México > ]
```

---

# Ejemplo con valor inicial

```html
<plus-country
name="country"
value="ES">
</plus-country>
```

Resultado:

```
🇪🇸 Spain
```

---

# Ejemplo en formulario

```html
<form>

  <plus-country
  name="country"
  requerid>
  </plus-country>

  <button type="submit">
    Save
  </button>

</form>
```

El formulario enviará:

```
country=MX
```

---

# Ventajas

`PlusCountry` simplifica mucho el desarrollo:

* evita escribir manualmente la lista de países
* incluye banderas automáticamente
* usa códigos ISO estándar
* reutiliza `PlusSelect`
* compatible con formularios
* soporta traducciones
* fácil de usar

---

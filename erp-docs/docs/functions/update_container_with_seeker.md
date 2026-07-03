# `update_container_with_seeker()`

La función `update_container_with_seeker()` permite crear búsquedas dinámicas dentro del ERP, actualizando automáticamente una tabla o contenedor conforme el usuario escribe o modifica los filtros disponibles.

Su objetivo es simplificar la implementación de listados con búsqueda, filtros y paginación, evitando tener que programar manualmente toda la lógica de comunicación con el servidor.

---

## Sintaxis

```javascript
update_container_with_seeker(
    inputsId,
    fieldId,
    divHtml,
    searchUrl,
    method = "POST",
    loadingImage = null,
    delay = 500,
    type = "tr"
);
```

---

## Parámetros

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `inputsId` | Array \| String | Campo de búsqueda principal y filtros adicionales. |
| `fieldId` | String | Contenedor donde se mostrarán los resultados. |
| `divHtml` | String | Plantilla HTML utilizada para renderizar cada elemento. |
| `searchUrl` | String | Ruta del servidor que realizará la búsqueda. |
| `method` | String | Método HTTP utilizado (`POST` o `GET`). |
| `loadingImage` | String \| Boolean | Imagen opcional para mostrar cuando no existan resultados o durante la carga. |
| `delay` | Number | Tiempo de espera antes de realizar la búsqueda (milisegundos). |
| `type` | String | Tipo de contenedor que será actualizado. |

---

## Ejemplo

```javascript
update_container_with_seeker(
    ["searchProduct", "categoryFilter"],
    "tableProducts",
    productTemplate,
    "/products/search"
);
```

---

## ¿Qué realiza esta función?

- Realiza búsquedas dinámicas mientras el usuario escribe.
- Permite utilizar múltiples filtros de búsqueda.
- Consulta la información al servidor mediante AJAX.
- Actualiza automáticamente el contenido del contenedor.
- Genera la paginación de los resultados.
- Muestra mensajes cuando no existen registros.
- Traduce automáticamente el contenido generado dinámicamente.
- Evita enviar solicitudes innecesarias utilizando un tiempo de espera (*debounce*).

!!! note

    Esta función es el componente estándar del ERP para construir tablas y listados con búsqueda en tiempo real, filtros y paginación de forma automática.
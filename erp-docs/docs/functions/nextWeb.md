# `nextWeb()`

La función `nextWeb()` permite realizar la navegación entre las diferentes páginas del ERP sin recargar completamente el navegador con un estilo SPA.

Su objetivo es ofrecer una experiencia más rápida y fluida, cargando dinámicamente el contenido, los estilos, los scripts y las traducciones de la página solicitada.

---

## Sintaxis

```javascript
await nextWeb(url)
```

---

## Parámetros

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `url` | String | Ruta de la página que se desea cargar. |

---

## Ejemplo
```javascript
await nextWeb("/inventario/productos/");
```

```javascript
await nextWeb("/inventario/ver_productos/1/");
```

---

## ¿Qué realiza esta función?

- Carga la página solicitada de forma dinámica.
- Actualiza el contenido principal del ERP.
- Carga automáticamente los archivos CSS y JavaScript necesarios.
- Aplica las traducciones correspondientes a la aplicación.
- Mantiene el historial de navegación del usuario.
- Evita recargar completamente el navegador, mejorando el rendimiento y la experiencia de uso.

!!! note

    Esta función es utilizada internamente por el ERP en el frontend para cambiar entre módulos y aplicaciones sin necesidad de realizar una recarga completa de la página.
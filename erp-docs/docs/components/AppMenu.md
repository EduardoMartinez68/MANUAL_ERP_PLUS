# AppMenu

`AppMenu` es un **Web Component** que genera automáticamente un **menú de navegación de submódulos** dentro del ERP.

Este componente permite definir elementos de menú usando etiquetas `<sub-menu>` en HTML y convertirlas dinámicamente en un **menú de navegación estructurado** con botones y enlaces.

El objetivo principal es **simplificar la creación de menús de aplicaciones** y mantener una **estructura consistente** en la interfaz del sistema.

---

## Uso básico

El programador solo necesita definir el menú usando la etiqueta `<app-menu>` y sus elementos `<sub-menu>`.

```html id="appmenu-basic"
<app-menu>

  <sub-menu 
      name="Customers"
      link="/customers">
  </sub-menu>

  <sub-menu 
      name="Orders"
      link="/orders">
  </sub-menu>

</app-menu>
```

El componente generará automáticamente la estructura del menú.

---

## Atributos de `<sub-menu>`

| Atributo | Descripción                                    |
| -------- | ---------------------------------------------- |
| `name`   | Nombre del submenú que se mostrará en el botón |
| `link`   | Ruta o página que se abrirá al hacer clic      |
| `class`  | Clase CSS adicional para el botón              |

---

## Ejemplo completo

```html id="appmenu-full-example"
<app-menu>

  <sub-menu
      name="menu.products"
      link="/products"
      class="btn-navbar-primary">
  </sub-menu>

  <sub-menu
      name="menu.orders"
      link="/orders"
      class="btn-navbar-secondary">
  </sub-menu>

  <sub-menu
      name="menu.customers"
      link="/customers">
  </sub-menu>

</app-menu>
```

---

## HTML generado

El componente transforma la estructura en un menú similar a:

```html id="generated-app-menu"
<nav class="sub-menu-app-nav">

  <div class="sub-menu-app-nav-container">

    <div class="sub-menu-app-nav-item">
      <a class="sub-menu-app-nav-link sub-menu-app-link-base" onclick="nextWeb('/products')">
        <button class="btn btn-navbar-primary">Products</button>
      </a>
    </div>

    <div class="sub-menu-app-nav-item">
      <a class="sub-menu-app-nav-link sub-menu-app-link-base" onclick="nextWeb('/orders')">
        <button class="btn btn-navbar-secondary">Orders</button>
      </a>
    </div>

  </div>

</nav>
```

---

## Sistema de traducción

El atributo `name` se procesa usando la función:

```javascript id="translate-example"
translate_text(nameSubMenu)
```

Esto permite usar claves de traducción en lugar de texto plano.

Ejemplo:

```html id="translation-example"
<sub-menu name="menu.products" link="/products"></sub-menu>
```

Si el diccionario contiene:

```javascript id="translation-dictionary"
LANG["menu.products"] = "Products";
```

El botón mostrará:

```text id="translated-result"
Products
```

---

## Navegación

El componente utiliza la función:

```javascript id="nextweb-call"
nextWeb(link)
```

Esto permite que el ERP controle internamente la navegación entre páginas o módulos.

Ejemplo generado:

```html id="navigation-example"
<a onclick="nextWeb('/orders')">
```

---

## Comportamiento

Cuando el componente se monta en el DOM:

1. Crea un contenedor `<nav>` principal.
2. Crea un contenedor interno para los elementos del menú.
3. Busca todos los elementos `<sub-menu>` dentro del componente.
4. Para cada `<sub-menu>`:

   * Obtiene el nombre del menú (`name`).
   * Traduce el texto usando `translate_text`.
   * Obtiene la ruta (`link`).
   * Obtiene las clases CSS del botón.
5. Genera la estructura HTML del botón y el enlace.
6. Inserta los elementos en el contenedor del menú.
7. Reemplaza la etiqueta `<app-menu>` original con el menú generado.

---

## Código fuente

```javascript id="source-appmenu"
class AppMenu extends HTMLElement {
  constructor() {
    super();
  }

  connectedCallback() {

    const container = document.createElement('nav');
    container.className = 'sub-menu-app-nav';

    const inner = document.createElement('div');
    inner.className = 'sub-menu-app-nav-container';

    const items = this.querySelectorAll('sub-menu');

    items.forEach(item => {

      const nameSubMenu = item.getAttribute('name') || '';
      const name = translate_text(nameSubMenu);

      const link = item.getAttribute('link') || '#';
      const btnClass = item.getAttribute('class') || 'btn-navbar';

      const navItem = document.createElement('div');
      navItem.className = 'sub-menu-app-nav-item';

      const a = document.createElement('a');
      a.className = 'sub-menu-app-nav-link sub-menu-app-link-base';
      a.setAttribute('onclick', `nextWeb('${link}')`);

      const button = document.createElement('button');
      button.className = `btn ${btnClass}`;
      button.setAttribute('t', name);
      button.textContent = name;

      a.appendChild(button);
      navItem.appendChild(a);
      inner.appendChild(navItem);
    });

    container.appendChild(inner);
    this.replaceWith(container);
  }
}
```

---

## Flujo del componente

```text id="appmenu-flow"
HTML
 ↓
app-menu
 ↓
Busca elementos sub-menu
 ↓
Traduce textos
 ↓
Genera botones y enlaces
 ↓
Construye el menú
 ↓
Reemplaza el componente
```

---

## Ventajas

* Simplifica la creación de menús en el ERP
* Permite definir la navegación usando HTML simple
* Soporte automático para traducciones
* Estructura consistente en todos los módulos
* Reduce duplicación de código
* Facilita el mantenimiento del sistema

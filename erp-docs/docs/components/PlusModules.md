# PlusModules y PlusModule

`PlusModules` y `PlusModule` son **Web Components** que trabajan juntos para crear un **sistema de módulos con sidebar**.

Este componente permite construir una interfaz donde:

* A la **izquierda** aparece una lista de módulos.
* A la **derecha** aparece el contenido del módulo seleccionado.
* Cada módulo puede tener **icono, nombre, descripción y contenido personalizado**.
* También puede ejecutar una **función cuando el módulo se vuelve visible**.

Este sistema es ideal para **dashboards, configuraciones, paneles administrativos o secciones de ERP**.

---

# Estructura básica

El programador solo necesita escribir esto en HTML:

```html
<plus-modules col="4">

  <plus-module
    name="General"
    icon="fi-rr-settings"
    desc="General configuration">

    <h3>General settings</h3>
    <p>Content of the module</p>

  </plus-module>

  <plus-module
    name="Users"
    icon="fi-rr-users"
    desc="Manage users">

    <h3>User management</h3>
    <p>Users list...</p>

  </plus-module>

</plus-modules>
```

El componente automáticamente generará:

```
Sidebar (módulos)  |  Contenido del módulo
-------------------|----------------------
General            |  General settings
Users              |
```

---

# Atributo `col`

El atributo `col` define el tamaño del **sidebar** usando un sistema de **12 columnas**.

```
col="4"
```

Esto significa:

```
Sidebar = 4 columnas
Contenido = 8 columnas
```

Cálculo interno:

```javascript
const colSidebar = Math.max(1, Math.min(col, 11));
const colContent = 12 - colSidebar;
```

Esto evita valores inválidos.

---

# Estructura generada

El componente crea dos contenedores principales:

```
plus-modules
│
├── sidebar
│   ├── módulo 1
│   ├── módulo 2
│   └── módulo 3
│
└── content
    ├── panel módulo 1
    ├── panel módulo 2
    └── panel módulo 3
```

---

# Sidebar de módulos

Cada `<plus-module>` genera un elemento en el sidebar.

El sidebar contiene:

* Icono
* Nombre del módulo
* Descripción
* Flecha indicadora

Ejemplo visual:

```
⚙ General
   General configuration >

👥 Users
   Manage users >
```

El icono usa la librería:

```
fi (Flat Icons)
```

Ejemplo:

```
fi-rr-users
```

---

# Sistema de traducción

Los nombres de los módulos se traducen automáticamente usando:

```javascript
translate_text(name)
```

Ejemplo:

```
name="settings.general"
```

Si el diccionario contiene:

```
settings.general = "Configuración general"
```

Se mostrará:

```
Configuración general
```

---

# Contenido de módulos

El contenido dentro de `<plus-module>` se mueve al panel correspondiente.

Ejemplo:

```html
<plus-module name="Users">

  <table>
    <tr>
      <td>User 1</td>
    </tr>
  </table>

</plus-module>
```

El componente lo transforma en:

```
Panel del módulo Users
```

Internamente mueve los nodos usando:

```javascript
while (m.firstChild) {
  panel.appendChild(m.firstChild);
}
```

---

# Cambio de módulos

Cuando el usuario hace click en un módulo:

1. Se marca el módulo como **activo**
2. Se ocultan todos los paneles
3. Se muestra el panel correspondiente

Código:

```javascript
panels.forEach((p, j) => {
  p.style.display = j == index ? 'block' : 'none';
});
```

---

# Función `on-visible`

Cada módulo puede ejecutar una función cuando se vuelve visible.

Ejemplo:

```html
<plus-module
  name="Schedules"
  on-visible="initSchedules(2)">
</plus-module>
```

Cuando el módulo se abre, ejecuta:

```javascript
initSchedules(2)
```

Internamente se ejecuta usando:

```javascript
new Function(scriptStr)
```

Esto permite ejecutar **código dinámico**.

---

# Ejecución automática del primer módulo

Cuando el componente se carga:

```javascript
triggerVisibleFunction(0);
```

Esto ejecuta la función del **primer módulo** automáticamente.

---

# Sistema responsive

El componente incluye soporte completo para **móviles**.

### Pantallas grandes

```
Sidebar | Content
```

### Pantallas pequeñas (<768px)

Primero se muestra el **sidebar**.

```
Modules list
```

Cuando se selecciona un módulo:

```
Module content
[X]
```

---

# Botón de cierre en móvil

En móvil aparece un botón:

```
[X]
```

Este botón permite volver al sidebar.

Código:

```javascript
closeBtn.addEventListener('click', () => {
  showSidebarMobile();
});
```

---

# Optimización en resize

El sistema detecta cambios de tamaño de pantalla.

Para evitar cálculos excesivos:

```javascript
if (Math.abs(newWidth - lastWidth) < 50) return;
```

Esto evita ejecutar lógica si el cambio de tamaño es muy pequeño.

---

# Flujo del componente

```
plus-modules
      ↓
Busca plus-module
      ↓
Crea sidebar
      ↓
Crea paneles de contenido
      ↓
Mueve el contenido al panel
      ↓
Activa primer módulo
      ↓
Espera interacción del usuario
```

---

# Componente `PlusModule`

`PlusModule` es el componente que representa **cada módulo individual**.

Su función principal es agregar una clase CSS.

```javascript
class PlusModule extends HTMLElement {
  connectedCallback() {
    this.classList.add('plus-modules-module');
  }
}
```

Esto permite aplicar estilos como:

```
.plus-modules-module
```

---

# Ejemplo completo

```html
<plus-modules col="3">

  <plus-module
    name="General"
    icon="fi-rr-settings"
    desc="General settings"
    on-visible="initGeneral()">

    <h2>General</h2>

  </plus-module>

  <plus-module
    name="Users"
    icon="fi-rr-users"
    desc="Manage users"
    on-visible="loadUsers()">

    <h2>Users</h2>

  </plus-module>

  <plus-module
    name="Permissions"
    icon="fi-rr-shield"
    desc="User permissions">

    <h2>Permissions</h2>

  </plus-module>

</plus-modules>
```

---

# Ventajas

* Sistema modular reutilizable
* Sidebar automático
* Soporte para iconos
* Soporte para descripciones
* Ejecución de funciones al abrir módulos
* Responsive automático
* Separación clara entre navegación y contenido
* Ideal para dashboards y ERP
* Reduce mucho código HTML y JavaScript

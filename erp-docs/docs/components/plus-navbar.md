# PlusNavbar

`<plus-navbar>` es un componente de navegación que forma parte del sistema **PLUS UI**.  
Permite crear barras de navegación dinámicas con soporte para **submenús jerárquicos**, iconos y eventos personalizados.

Este componente fue diseñado específicamente para el ERP con el objetivo de ofrecer:

- Navegación rápida
- Estructura clara de menús
- Soporte para submenús ilimitados
- Integración directa con el sistema de rutas del ERP

El componente utiliza **Web Components nativos**, encapsulando su estructura y estilos mediante **Shadow DOM**.

---

# Uso básico

```html
<plus-navbar>

    <option 
        t="Dashboard"
        icon="fa-solid fa-chart-line"
        link="/dashboard">
    </option>

    <option 
        t="Ventas"
        icon="fa-solid fa-cash-register">

        <option 
            t="Nueva venta"
            link="/ventas/nueva">
        </option>

        <option 
            t="Historial de ventas"
            link="/ventas/historial">
        </option>

    </option>

</plus-navbar>
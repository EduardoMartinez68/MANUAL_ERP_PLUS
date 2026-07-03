# Arquitectura del ERP

La arquitectura de **PLUS ERP** fue diseñada siguiendo una filosofía **modular**, donde cada componente del sistema es independiente y puede evolucionar de forma aislada.

El objetivo principal de esta arquitectura es que las actualizaciones del sistema sean simples, seguras y escalables, permitiendo actualizar únicamente el **núcleo del ERP** sin afectar las aplicaciones desarrolladas sobre él.

Cada aplicación posee su propio ciclo de vida, configuración, servicios, vistas, traducciones y extensiones, facilitando el mantenimiento y el desarrollo de nuevas funcionalidades.

!!! info "Objetivos de la arquitectura"

    - Arquitectura completamente modular.
    - Actualización independiente del núcleo.
    - Aplicaciones desacopladas.
    - Fácil mantenimiento.
    - Mayor organización del código.
    - Escalabilidad para proyectos grandes.
    - Sistema preparado para plugins y extensiones.

---

# Estructura del proyecto

La estructura general del proyecto es la siguiente:

```text
PLUS ERP/
├── createApp.py
├── createPlugin.py
├── apps/
│   └── my_app/
│       ├── config.yaml
│       ├── config/
│       ├── links/
│       ├── locale/
│       ├── services/
│       ├── static/
│       ├── views/
│       └── models.py
│
├── core/
├── media/
├── static/
├── .env
└── manager.py
```

---

# Directorios principales

## `apps/`

Contiene todas las aplicaciones instaladas dentro del ERP.

Cada aplicación es completamente independiente del resto del sistema y contiene todo lo necesario para funcionar.

Ejemplo:

```text
apps/
└── inventario/
```

```text
apps/
└── ventas/
```

```text
apps/
└── compras/
```

Cada una puede tener sus propios modelos, vistas, traducciones, servicios y plugins.

---

## `core/`

Es el núcleo del ERP.

Aquí se encuentra toda la lógica principal del framework interno.

Este directorio rara vez necesita ser modificado por los desarrolladores de aplicaciones.

Cuando existe una nueva versión del sistema normalmente **únicamente se reemplaza este directorio**, permitiendo actualizar el ERP sin modificar las aplicaciones instaladas.

---

## `static/`

Archivos estáticos compartidos por todo el sistema.

Por ejemplo:

- CSS
- JavaScript
- Imágenes
- Iconos
- Fuentes

---

## `media/`

Almacena los archivos cargados por los usuarios.

Por ejemplo:

- Fotografías
- Documentos
- PDFs
- Facturas
- Archivos adjuntos

---

## `.env`

Contiene las variables de entorno del proyecto.

Ejemplos:

- Configuración de la base de datos.
- Claves secretas.
- Configuración SMTP.
- Tokens.
- Configuración de producción.

---

## `manager.py`

Punto de entrada principal del sistema.

Desde este archivo se ejecutan las tareas administrativas del ERP.

---

# Scripts del núcleo

El proyecto incorpora herramientas que permiten automatizar gran parte del desarrollo.

## `createApp.py`

Genera automáticamente la estructura base de una nueva aplicación.

Esto evita crear manualmente todos los directorios necesarios.

Ejemplo:

```bash
python createApp.py inventario
```

Resultado:

```text
apps/
└── inventario/
    ├── config/
    ├── links/
    ├── locale/
    ├── services/
    ├── static/
    ├── views/
    ├── models.py
    └── config.yaml
```

---

## `createPlugin.py`

Genera la estructura base para crear un nuevo plugin o extensión del sistema.

De esta forma todas las extensiones mantienen la misma organización y convenciones del proyecto.

---

# Arquitectura de una aplicación

Cada aplicación sigue exactamente la misma estructura.

```text
mi_app/
├── config.yaml
├── config/
├── links/
├── locale/
├── services/
├── static/
├── views/
└── models.py
```

## `config.yaml`

Archivo principal de configuración de la aplicación.

Aquí se registran datos como:

- Nombre
- Versión
- Dependencias
- Plugins
- Configuración general

---

## `config/`

Configuraciones específicas de la aplicación. Todos estos archivos son creados con el nucleo del ERP cuando se inicia. 

---

## `links/`

Define las rutas (URLs) pertenecientes exclusivamente a esta aplicación.

El núcleo del ERP detecta automáticamente estos archivos durante el inicio del sistema. Para acceder a un link seria usando el nombre de la app con la extension de la vista programada ejemplo:

**/inventario/ver_inventario/**

---

## `locale/`

Archivos de internacionalización.

Permiten traducir la aplicación a distintos idiomas. Esta carpeta contendra sub carpetas con las iniciales del idioma ejemplo es, pl, etc y dentro de la carpeta su archivo  **translate.json**. Normalmente en json estara estructurado como NombreApp.etiqueta.texto ejemplo en español
```json
{
    "inventory.title.home": "Inicio"
}
```

---

## `services/`

Aquí reside toda la lógica de negocio.

Este directorio representa una de las piezas más importantes de la arquitectura.

Toda la lógica relacionada con procesos, reglas del negocio y operaciones debe implementarse aquí.

Ejemplos:

- Gestión de inventario.
- Facturación.
- Cálculo de impuestos.
- Validaciones.
- Procesos automáticos.
- Integraciones con APIs.

!!! tip "Buenas prácticas"

    Las vistas deben mantenerse lo más simples posible.
    Toda la lógica del negocio debe implementarse dentro de **services/**.

---

## `links/`

Contiene las vistas de la aplicación. Su archivo principal es **views.py**

Su responsabilidad principal es:

- Procesar la petición.
- Llamar a los servicios necesarios.
- Devolver la respuesta correspondiente.

Las vistas no deberían contener lógica compleja del negocio.

---

## `views/`

Contiene las vistas html de la aplicación.

Su responsabilidad principal es:

- Ser la interfaz del usuario.
- Tener logica de peticion con APIs al sistema ERP.

---

## `models.py`

Define los modelos de base de datos utilizados por la aplicación. 

---

## `static/`

Recursos estáticos exclusivos de la aplicación.

Por ejemplo:

- CSS
- JavaScript
- Imágenes
- Iconos

---

# Inicio del sistema

Cuando el servidor inicia, el núcleo del ERP ejecuta automáticamente un proceso de descubrimiento de aplicaciones.

El flujo general es el siguiente:

```text
Inicio del servidor
        │
        ▼
Buscar aplicaciones instaladas
        │
        ▼
Leer config.yaml
        │
        ▼
Detectar plugins
        │
        ▼
Registrar rutas
        │
        ▼
Registrar vistas
        │
        ▼
Inicializar servicios
        │
        ▼
ERP listo
```

Este mecanismo evita registrar manualmente cada aplicación dentro del núcleo del proyecto.

---

# Descubrimiento automático

Una de las características más importantes de PLUS ERP es el **descubrimiento automático de módulos**.

El sistema inspecciona el directorio `apps/` y registra automáticamente:

- Aplicaciones.
- Plugins.
- URLs.
- Vistas.
- Configuraciones.
- Servicios.

Gracias a este enfoque, agregar una nueva aplicación normalmente consiste únicamente en copiarla dentro del directorio `apps/`.

No es necesario modificar el núcleo del ERP.

---

# Ventajas de esta arquitectura

✅ Código mucho más limpio.

✅ Menor acoplamiento entre módulos.

✅ Fácil mantenimiento.

✅ Actualizaciones más sencillas.

✅ Desarrollo de aplicaciones independiente.

✅ Plugins desacoplados.

✅ Escalable para proyectos empresariales.

✅ Mayor reutilización del código.

✅ Facilita el trabajo en equipos grandes.

---

# Filosofía del proyecto

PLUS ERP busca mantener una separación clara entre el núcleo del sistema y las aplicaciones desarrolladas sobre él.

El objetivo es que los desarrolladores puedan concentrarse en la lógica del negocio sin preocuparse por la infraestructura interna del ERP.

Cada aplicación funciona como un módulo independiente, mientras que el núcleo se encarga automáticamente de descubrirla, configurarla y ponerla en funcionamiento.

Esta arquitectura permite que el sistema permanezca limpio, organizado y preparado para crecer durante muchos años sin comprometer su mantenibilidad.
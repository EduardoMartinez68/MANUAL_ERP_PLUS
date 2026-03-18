# set_status_plus_switch

`set_status_plus_switch` es una **función auxiliar (helper)** que permite **cambiar el estado de un componente `plus-switch` desde JavaScript** usando su `id`.

Esta función sirve para **activar o desactivar un switch de forma programática**, sin que el usuario tenga que hacer clic manualmente.

En resumen:

**`set_status_plus_switch` = cambia el estado ON/OFF de un `plus-switch`.**

---

# Código fuente

```javascript
function set_status_plus_switch(id, newValue) {
  const mySwitch = document.getElementById(id);
  if (!mySwitch) return;

  if (typeof mySwitch.setChecked === 'function') {
    mySwitch.setChecked(newValue);
  }
}
```

---

# Qué hace esta función

La función sigue tres pasos principales:

```
1. Buscar el componente en el DOM
2. Verificar que tenga el método setChecked()
3. Cambiar el estado del switch
```

Esto permite **controlar el switch desde el código JavaScript**.

---

# Parámetros

La función recibe **dos parámetros**.

---

# id

Es el `id` del componente `plus-switch`.

```javascript
id
```

Debe coincidir con el atributo `id` definido en el HTML.

Ejemplo:

```html
<plus-switch id="notifications"></plus-switch>
```

Uso:

```
notifications
```

---

# newValue

Define el **nuevo estado del switch**.

```javascript
newValue
```

Debe ser un **valor booleano**.

Valores posibles:

```
true  → activar el switch
false → desactivar el switch
```

---

# Paso 1 — Buscar el componente

Primero se busca el componente en el DOM.

```javascript
const mySwitch = document.getElementById(id);
```

Ejemplo:

```
id = "notifications"
```

Resultado:

```html
<plus-switch id="notifications">
```

Si el componente no existe, la función termina.

```javascript
if (!mySwitch) return;
```

Esto evita errores como:

```
Cannot read properties of null
```

---

# Paso 2 — Verificar el método setChecked

El componente `PlusSwitch` tiene un método público llamado:

```
setChecked()
```

La función verifica que el método exista.

```javascript
if (typeof mySwitch.setChecked === 'function')
```

Esto asegura que el elemento realmente sea un `plus-switch`.

---

# Paso 3 — Cambiar el estado del switch

Si todo es correcto, se ejecuta:

```javascript
mySwitch.setChecked(newValue);
```

Esto cambia el estado interno del switch.

Resultado:

```
true  → switch activado
false → switch desactivado
```

---

# Ejemplo básico

HTML:

```html
<plus-switch
id="notifications"
text="Notifications">
</plus-switch>
```

JavaScript:

```javascript
set_status_plus_switch("notifications", true);
```

Resultado:

```
Switch activado
```

---

# Ejemplo para desactivar

```javascript
set_status_plus_switch("notifications", false);
```

Resultado:

```
Switch apagado
```

---

# Ejemplo con botón

HTML:

```html
<button onclick="activar()">Activar</button>

<plus-switch
id="notifications"
text="Notifications">
</plus-switch>
```

JavaScript:

```javascript
function activar(){
  set_status_plus_switch("notifications", true);
}
```

Resultado:

```
Al presionar el botón el switch se activa
```

---

# Ejemplo con datos del servidor

```javascript
fetch("/api/settings")
  .then(r => r.json())
  .then(data => {

    set_status_plus_switch(
      "notifications",
      data.notifications
    );

  });
```

Respuesta del servidor:

```json
{
 "notifications": true
}
```

Esto actualizará el switch automáticamente.

---

# Ejemplo al cargar un formulario

```javascript
function loadSettings(settings){

  set_status_plus_switch(
    "autoSave",
    settings.auto_save
  );

}
```

Esto permite **cargar configuraciones guardadas**.

---

# Comparación con acceso directo

Sin helper:

```javascript
const sw = document.getElementById("notifications");

if(sw && typeof sw.setChecked === "function"){
  sw.setChecked(true);
}
```

Con helper:

```javascript
set_status_plus_switch("notifications", true);
```

Mucho más limpio.

---

# Flujo de funcionamiento

```
set_status_plus_switch()
        ↓
buscar componente por id
        ↓
verificar método setChecked
        ↓
cambiar estado del switch
```

---

# Cuándo usar esta función

Es útil cuando:

* se cargan configuraciones guardadas
* se actualiza la interfaz desde una API
* se resetean formularios
* se controla el estado de un switch desde botones
* se sincroniza el estado con otros componentes

---

# Ventajas

Usar esta función tiene varias ventajas:

* simplifica el código
* evita errores si el elemento no existe
* usa la API oficial del componente
* mejora la legibilidad
* es reutilizable

---

# Conclusión

`set_status_plus_switch` es un **helper simple pero muy útil para controlar un `plus-switch` desde JavaScript**.

Permite **activar o desactivar el switch de forma segura y reutilizable**, facilitando la integración del componente dentro de formularios, configuraciones y aplicaciones dinámicas.

# PLUS Global Registry

The **PLUS Global Registry** is a module system used by PLUS ERP to encapsulate JavaScript code and avoid conflicts between different modules.

Every JavaScript file has its own **namespace**, automatically generated from its file path. This allows different modules to define functions and variables with the same name without causing collisions.

For example:

```text
apps/customers/views/form.html
```

becomes

```text
apps_customers_views_form
```

This namespace is automatically injected by the backend into the following variable:

```javascript
const namespace = "{plus}";
```

When the page is rendered, it becomes:

```javascript
const namespace = "apps_customers_views_form";
```

---

# Why does PLUS use namespaces?

Imagine two different modules:

```text
apps/customers/views/form.html
apps/products/views/form.html
```

Both modules can have a function named:

```javascript
create();
```

Without namespaces, JavaScript would not know which function to execute.

With the PLUS registry, both functions are isolated:

```javascript
Plus.Functions.get("apps_customers_views_form").create();

Plus.Functions.get("apps_products_views_form").create();
```

Each module is completely independent.

---

# Plus.Functions

`Plus.Functions` stores JavaScript functions so they can be executed from any script inside the ERP.

---

## Registering functions

Use `define()` to register a function.

### Syntax

```javascript
Plus.Functions.define(namespace, name, fn);
```

---

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `namespace` | String | Unique identifier of the JavaScript module. Usually `{plus}`. |
| `name` | String | Function name used inside the module. |
| `fn` | Function | Function to register. |

---

## Example

```javascript
const namespace = "{plus}";

function create() {
    console.log("Customer created");
}

Plus.Functions.define(namespace, "create", create);
```

---

# Executing functions

Functions can be executed from any JavaScript file.

### Syntax

```javascript
Plus.Functions.get(namespace).functionName();
```

---

### Example

```javascript
Plus.Functions.get(namespace).create();
```

or

```javascript
Plus.Functions.get("apps_customers_views_form").create();
```

---

# Multiple functions

A module can register as many functions as necessary.

```javascript
function create(){}

function update(){}

function remove(){}

Plus.Functions.define(namespace, "create", create);

Plus.Functions.define(namespace, "update", update);

Plus.Functions.define(namespace, "remove", remove);
```

Later:

```javascript
Plus.Functions.get(namespace).create();

Plus.Functions.get(namespace).update();

Plus.Functions.get(namespace).remove();
```

---

# Resetting functions

Deletes registered functions.

### Delete one module

```javascript
Plus.Functions.reset(namespace);
```

### Delete all modules

```javascript
Plus.Functions.reset();
```

---

# Plus.Variables

`Plus.Variables` stores variable references instead of values.

Because a reference is stored, every call to `get()` returns the latest value.

---

## Why store references?

Suppose a variable changes many times.

```javascript
let counter = 0;
```

If only the value were stored, the registry would always return:

```text
0
```

Instead, PLUS stores a reference:

```javascript
{
    value: counter
}
```

Whenever `value` changes, the registry automatically returns the new value.

---

# Registering variables

### Syntax

```javascript
Plus.Variables.define(namespace, variableName, referenceObject);
```

---

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `namespace` | String | Module namespace. |
| `variableName` | String | Variable identifier. |
| `referenceObject` | Object | Object containing the variable reference. |

---

## Example

```javascript
const namespace = "{plus}";

const patientId = {
    value: 15
};

Plus.Variables.define(namespace, "patientId", patientId);
```

---

# Reading variables

### Syntax

```javascript
Plus.Variables.get(namespace, variableName);
```

---

## Example

```javascript
Plus.Variables.get(namespace, "patientId");
```

Result

```text
15
```

---

If the value changes:

```javascript
patientId.value = 35;
```

the registry automatically returns:

```javascript
Plus.Variables.get(namespace, "patientId");
```

Result

```text
35
```

No registration is required again.

---

# Resetting variables

Deletes stored variables.

### Delete one module

```javascript
Plus.Variables.reset(namespace);
```

### Delete all variables

```javascript
Plus.Variables.reset();
```

---

# Complete example

```javascript
(() => {

    const namespace = "{plus}";

    const counter = {
        value: 0
    };

    function increase() {
        counter.value++;
        console.log(counter.value);
    }

    function reset() {
        counter.value = 0;
    }

    Plus.Functions.define(namespace, "increase", increase);

    Plus.Functions.define(namespace, "reset", reset);

    Plus.Variables.define(namespace, "counter", counter);

})();
```

Later, from another script:

```javascript
Plus.Functions.get(namespace).increase();

console.log(
    Plus.Variables.get(namespace, "counter")
);

Plus.Functions.get(namespace).reset();
```

---

# Best practices

- Always use the automatically generated `{plus}` namespace.
- Register only functions that need to be accessed by other modules.
- Keep private helper functions outside the registry.
- Register only shared variables in `Plus.Variables`.
- Use descriptive names such as `create`, `update`, `remove`, `render`, `load`, or `refresh`.
- Use `reset(namespace)` when a dynamic view is destroyed to free memory.
- Avoid directly modifying the internal `modules` or `registry` objects.

---

# Architecture

Every JavaScript file acts as an independent module.

```text
apps/
│
├── customers/
│   └── views/
│       └── form.html
│           namespace:
│           apps_customers_views_form
│
├── products/
│   └── views/
│       └── form.html
│           namespace:
│           apps_products_views_form
│
└── sales/
    └── views/
        └── invoice.html
            namespace:
            apps_sales_views_invoice
```

Internally, the registry stores the information in a structure similar to:

```javascript
Plus.Functions = {

    modules: {

        apps_customers_views_form: {
            create(){},
            update(){},
            render(){}
        },

        apps_products_views_form: {
            create(){},
            remove(){}
        }

    }

}
```

This architecture allows every module in PLUS ERP to remain completely encapsulated, preventing global name collisions while still enabling controlled communication between modules.

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
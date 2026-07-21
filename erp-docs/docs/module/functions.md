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

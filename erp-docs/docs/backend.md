# Backend del ERP

El backend está desarrollado en **Python**.

---

## Modelo de Producto

```python
class Product:

    table = "products"

    def save(self):
        db.insert(self.table,self)
```

---

## Controlador

```python
class ProductController:

    def list(self):

        products = Product.get_all()

        return json(products)
```
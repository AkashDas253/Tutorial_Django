## **Models in Django**

Models define the structure and behavior of the application's data and are the interface to the database in Django's **MTV architecture**. Each model typically maps to a single database table.

---

### **1. Purpose**

* Define data schema.
* Provide Pythonic access to database tables.
* Manage queries using Django ORM.

---

### **2. Creating a Model**

```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    in_stock = models.BooleanField(default=True)
    created_at = models.DateTimeField(auto_now_add=True)
```

Each attribute becomes a database column.

---

### **3. Field Types**

| Field Type                                 | Description                  |
| ------------------------------------------ | ---------------------------- |
| `CharField(max_length)`                    | Text field with length limit |
| `TextField()`                              | Large text                   |
| `IntegerField()`                           | Whole numbers                |
| `FloatField()`                             | Floating-point numbers       |
| `DecimalField(max_digits, decimal_places)` | Fixed-precision decimals     |
| `BooleanField()`                           | True/False                   |
| `DateTimeField(auto_now_add/auto_now)`     | Timestamp                    |
| `EmailField()`                             | Validates email format       |
| `FileField()` / `ImageField()`             | File or image upload         |

---

### **4. Meta Class**

Customize model behavior with an inner `Meta` class.

```python
class Meta:
    ordering = ['-created_at']
    verbose_name = "Product Item"
    db_table = 'product_table'
```

---

### **5. Model Methods**

Add business logic related to the model.

```python
def is_expensive(self):
    return self.price > 100
```

---

### **6. String Representation**

Recommended to define `__str__()`:

```python
def __str__(self):
    return self.name
```

---

### **7. Relationships**

| Type         | Field             | Description                              |
| ------------ | ----------------- | ---------------------------------------- |
| One-to-many  | `ForeignKey`      | Many rows related to one parent          |
| One-to-one   | `OneToOneField`   | One row to one row                       |
| Many-to-many | `ManyToManyField` | Multiple rows related to multiple others |

Example:

```python
class Category(models.Model):
    name = models.CharField(max_length=100)

class Product(models.Model):
    category = models.ForeignKey(Category, on_delete=models.CASCADE)
```

---

### **8. Model Inheritance**

Use for reusable model structures.

```python
class BaseModel(models.Model):
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        abstract = True

class Product(BaseModel):
    name = models.CharField(max_length=100)
```

---

### **9. Model Managers**

Custom interfaces to model queries.

```python
class ProductManager(models.Manager):
    def available(self):
        return self.filter(in_stock=True)

class Product(models.Model):
    ...
    objects = ProductManager()
```

Usage:

```python
Product.objects.available()
```

---

### **10. Saving and Querying**

```python
product = Product(name="Book", price=9.99)
product.save()

products = Product.objects.all()
expensive = Product.objects.filter(price__gt=100)
```

---

### **11. Migrations**

* Create migrations: `python manage.py makemigrations`
* Apply migrations: `python manage.py migrate`

---

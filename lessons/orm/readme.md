## **ORM (Object-Relational Mapping) in Django**

Django’s ORM lets you interact with the database using Python code instead of raw SQL. It maps database tables to Python classes (models), rows to instances, and columns to attributes.

---

### **1. Model Classes**

Each model class maps to a single database table.

```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    in_stock = models.BooleanField(default=True)
```

---

### **2. Field Types**

| Field Type                    | Description              |
| ----------------------------- | ------------------------ |
| `CharField`                   | String (with max length) |
| `TextField`                   | Large text               |
| `IntegerField`                | Integer                  |
| `FloatField` / `DecimalField` | Numbers                  |
| `BooleanField`                | True/False               |
| `DateField` / `DateTimeField` | Date/time                |
| `EmailField`                  | Validates email          |
| `ForeignKey`                  | Many-to-One relationship |
| `ManyToManyField`             | Many-to-Many             |
| `OneToOneField`               | One-to-One               |

---

### **3. ORM Queries**

#### Create

```python
product = Product(name="Pen", price=10.99)
product.save()
# OR
Product.objects.create(name="Pen", price=10.99)
```

#### Retrieve

```python
Product.objects.all()
Product.objects.get(id=1)
Product.objects.filter(in_stock=True)
Product.objects.exclude(price=0)
Product.objects.order_by('-price')
```

#### Update

```python
product = Product.objects.get(id=1)
product.price = 12.99
product.save()
```

#### Delete

```python
product = Product.objects.get(id=1)
product.delete()
```

---

### **4. QuerySet Methods**

| Method                         | Description       |
| ------------------------------ | ----------------- |
| `.all()`                       | All rows          |
| `.filter(**kwargs)`            | WHERE clause      |
| `.get(**kwargs)`               | Single object     |
| `.exclude(**kwargs)`           | NOT condition     |
| `.order_by('field')`           | Sort              |
| `.values()` / `.values_list()` | Dicts/lists       |
| `.distinct()`                  | Remove duplicates |
| `.count()`                     | Row count         |
| `.exists()`                    | Boolean check     |
| `.first()` / `.last()`         | First/last record |

---

### **5. Lookups**

```python
Product.objects.filter(price__gt=10)
Product.objects.filter(name__icontains="pen")
Product.objects.filter(date__year=2024)
```

| Suffix                           | Meaning                    |
| -------------------------------- | -------------------------- |
| `__exact`                        | Equals (default)           |
| `__iexact`                       | Case-insensitive equals    |
| `__contains`                     | Substring                  |
| `__icontains`                    | Case-insensitive substring |
| `__gt`, `__lt`, `__gte`, `__lte` | Comparisons                |
| `__in`                           | IN clause                  |
| `__isnull`                       | NULL check                 |

---

### **6. Relationships**

#### ForeignKey

```python
class Category(models.Model):
    name = models.CharField(max_length=50)

class Product(models.Model):
    category = models.ForeignKey(Category, on_delete=models.CASCADE)
```

#### Reverse Access:

```python
category.product_set.all()
```

#### ManyToMany

```python
class Tag(models.Model):
    name = models.CharField(max_length=50)

class Product(models.Model):
    tags = models.ManyToManyField(Tag)
```

#### OneToOne

```python
class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
```

---

### **7. Meta Options**

Define table name, ordering, verbose names, etc.

```python
class Product(models.Model):
    ...

    class Meta:
        db_table = 'product_table'
        ordering = ['-price']
        verbose_name = 'Product Item'
```

---

### **8. Raw SQL Support**

```python
Product.objects.raw('SELECT * FROM myapp_product WHERE price > %s', [100])
```

---

### **9. Aggregations**

```python
from django.db.models import Avg, Max, Min, Count

Product.objects.aggregate(Avg('price'))
Product.objects.values('category').annotate(total=Count('id'))
```

---

### **10. Transactions**

```python
from django.db import transaction

with transaction.atomic():
    # atomic operations here
```

---

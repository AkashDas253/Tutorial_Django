### **Django & PostgreSQL Cheatsheet**  

PostgreSQL is a powerful, open-source database fully supported by Django.

---

## **1. Install PostgreSQL & Dependencies**  
```sh
pip install psycopg2-binary
```

---

## **2. Configure Django for PostgreSQL**  

### **Edit `settings.py`**
```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "your_db_name",
        "USER": "your_db_user",
        "PASSWORD": "your_db_password",
        "HOST": "localhost",
        "PORT": "5432",
    }
}
```

---

## **3. Create & Apply Migrations**
```sh
python manage.py makemigrations
python manage.py migrate
```

---

## **4. Connect to PostgreSQL Shell**
```sh
psql -U your_db_user -d your_db_name
```

---

## **5. PostgreSQL-Specific Django Features**

### **Auto-incrementing Primary Key (`BigAutoField`)**
```python
from django.db import models

class Product(models.Model):
    id = models.BigAutoField(primary_key=True)
```

### **PostgreSQL Array Field**
```python
from django.contrib.postgres.fields import ArrayField

class Product(models.Model):
    tags = ArrayField(models.CharField(max_length=50), default=list)
```

### **JSON Field**
```python
from django.contrib.postgres.fields import JSONField

class Product(models.Model):
    metadata = JSONField()
```

### **Full-Text Search**
```python
from django.contrib.postgres.search import SearchVector
from myapp.models import Product

Product.objects.annotate(search=SearchVector("name", "description")).filter(search="keyword")
```

---

## **6. Using Raw SQL Queries**
```python
from django.db import connection

with connection.cursor() as cursor:
    cursor.execute("SELECT * FROM myapp_product WHERE price > %s", [100])
    rows = cursor.fetchall()
```

---

## **7. Indexing for Performance**
```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100, db_index=True)  # Indexing column
```

---

## **8. Optimizing Queries with PostgreSQL**
| **Optimization** | **Django Feature** |
|-----------------|-------------------|
| Indexing | `db_index=True` |
| Bulk Insert | `Model.objects.bulk_create([...])` |
| Query Caching | `select_related()` (ForeignKey) |
| Lazy Loading | `prefetch_related()` (ManyToMany) |

Example:
```python
Product.objects.select_related("category").all()  # Optimized query
```

---

## **9. Handling Transactions**
```python
from django.db import transaction

@transaction.atomic
def update_product():
    Product.objects.filter(id=1).update(price=200)
```

---

## **10. PostgreSQL Extensions**
Enable **UUID Primary Keys**:
```python
import uuid
from django.db import models

class Product(models.Model):
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
```

---

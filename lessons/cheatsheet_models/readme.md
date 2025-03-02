### **Django Models Cheatsheet**  

#### **Key Features of Models**  
- Defines database schema as Python classes.  
- Inherits from `django.db.models.Model`.  
- Uses ORM (Object-Relational Mapping) for database operations.  

#### **Defining a Model**  
```python
from django.db import models

class MyModel(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
    created_at = models.DateTimeField(auto_now_add=True)
```

#### **Model Fields**  
| Field Type | Description |
|------------|------------|
| `CharField(max_length=n)` | String with max length `n`. |
| `TextField()` | Large text. |
| `IntegerField()` | Integer values. |
| `FloatField()` | Floating-point values. |
| `BooleanField()` | `True` or `False`. |
| `DateTimeField(auto_now_add=True)` | Stores timestamp when created. |
| `DateField(auto_now=True)` | Stores date, updates on modification. |
| `ForeignKey(Model, on_delete=models.CASCADE)` | One-to-many relation. |
| `ManyToManyField(Model)` | Many-to-many relation. |

#### **Model Meta Options**  
- Customizes model behavior.  
- Defined inside `class Meta:`.  

```python
class MyModel(models.Model):
    name = models.CharField(max_length=100)

    class Meta:
        ordering = ['name']  # Default ordering
        verbose_name = "Custom Name"
```

#### **Primary Key (PK) & Auto Fields**  
- `id` is created automatically as the primary key.  
- Can define manually using `primary_key=True`.  

```python
class CustomModel(models.Model):
    custom_id = models.AutoField(primary_key=True)
```

#### **Database Relationships**  
| Relationship | Field Type |
|-------------|------------|
| One-to-One | `OneToOneField(Model, on_delete=models.CASCADE)` |
| One-to-Many | `ForeignKey(Model, on_delete=models.CASCADE)` |
| Many-to-Many | `ManyToManyField(Model)` |

#### **CRUD Operations**  
- **Create:** `MyModel.objects.create(name="John", age=30)`  
- **Read:** `MyModel.objects.all()`, `MyModel.objects.filter(age=30)`  
- **Update:**  
  ```python
  obj = MyModel.objects.get(id=1)
  obj.age = 35
  obj.save()
  ```  
- **Delete:**  
  ```python
  obj = MyModel.objects.get(id=1)
  obj.delete()
  ```

#### **Model Methods**  
- Custom methods inside models for additional logic.  

```python
class MyModel(models.Model):
    name = models.CharField(max_length=100)

    def get_uppercase_name(self):
        return self.name.upper()
```

#### **QuerySet Methods**  
| Method | Description |
|--------|------------|
| `all()` | Returns all records. |
| `filter(condition)` | Filters records based on condition. |
| `exclude(condition)` | Excludes records matching condition. |
| `order_by(field)` | Orders records by field. |
| `count()` | Returns count of records. |
| `first()/last()` | Returns first or last record. |
| `values()` | Returns specific fields as dictionary. |

#### **Migrations**  
- **Create Migration:** `python manage.py makemigrations`  
- **Apply Migration:** `python manage.py migrate`  
- **View SQL:** `python manage.py sqlmigrate app_name migration_number`  

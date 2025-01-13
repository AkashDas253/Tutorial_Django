## Models in Django

Django models are the core of the Django ORM (Object-Relational Mapping). They define the structure of your database tables and allow interaction with database records through Python code.

---

### **Key Features of Models**

1. **Field Definitions**: Attributes in a model map to columns in a database table.
2. **Automatic Table Creation**: Django generates database tables based on models.
3. **Data Validation**: Enforces constraints such as `unique` and `max_length`.
4. **Query API**: Provides a rich API for database queries.
5. **Relations**: Supports relationships such as one-to-one, many-to-one, and many-to-many.

### Key Concepts of Django Models

1. **Model**: A Python class representing a database table.
2. **Fields**: Attributes of the model that map to columns in the database table.
3. **QuerySet**: A collection of database queries for retrieving objects.
4. **Meta Options**: Inner class for model metadata (e.g., table name, ordering).

---

### **Defining a Model**

```python
from django.db import models

class MyModel(models.Model):
    name = models.CharField(max_length=100, unique=True)
    age = models.IntegerField(default=18)
    email = models.EmailField(null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
```

---

### **Model Fields and Parameters**

| **Field Type**          | **Description**                                                                                     | **Parameters**                                                                                                        | **Default Value**            | **Range/Options**                                                                                        |
|--------------------------|-----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|------------------------------|----------------------------------------------------------------------------------------------------------|
| `CharField`             | A string field for small to large text.                                                            | `max_length`, `null`, `blank`, `unique`, `choices`, `default`, `db_index`                                            | `required=True`             | `max_length`: Integer (e.g., `1-255`)                                                                   |
| `TextField`             | A large text field for unlimited-length text.                                                     | `null`, `blank`, `default`, `db_index`                                                                               | `required=False`            | -                                                                                                        |
| `IntegerField`          | A field for integers.                                                                              | `null`, `blank`, `default`, `unique`, `db_index`                                                                     | `required=True`             | Integer range depends on database backend                                                                |
| `FloatField`            | A field for floating-point numbers.                                                               | `null`, `blank`, `default`, `unique`, `db_index`                                                                     | `required=True`             | Floating-point numbers                                                                                   |
| `BooleanField`          | A field for true/false values.                                                                     | `default`                                                                                                            | `required=True`             | `default`: `True` or `False`                                                                             |
| `DateField`             | A field for storing date values.                                                                  | `null`, `blank`, `auto_now`, `auto_now_add`                                                                          | `required=True`             | Dates in ISO format                                                                                      |
| `DateTimeField`         | A field for storing date and time values.                                                         | `null`, `blank`, `auto_now`, `auto_now_add`                                                                          | `required=True`             | ISO format date-time                                                                                     |
| `EmailField`            | A string field for email addresses.                                                              | `max_length`, `null`, `blank`, `unique`, `default`, `db_index`                                                       | `required=True`             | Valid email address                                                                                      |
| `URLField`              | A string field for URLs.                                                                          | `max_length`, `null`, `blank`, `unique`, `default`, `db_index`                                                       | `required=True`             | Valid URL                                                                                               |
| `FileField`             | A field for uploading files.                                                                      | `upload_to`, `null`, `blank`, `default`, `max_length`                                                                | `required=True`             | `upload_to`: Path where files will be saved                                                              |
| `ImageField`            | A `FileField` for images with validation.                                                        | `upload_to`, `null`, `blank`, `default`, `max_length`                                                                | `required=True`             | Image file formats                                                                                       |
| `ForeignKey`            | A field for one-to-many relationships.                                                           | `to`, `on_delete`, `related_name`, `related_query_name`, `null`, `blank`, `default`                                  | `required=True`             | `on_delete`: `models.CASCADE`, `models.PROTECT`, `models.SET_NULL`, `models.SET_DEFAULT`, `models.DO_NOTHING` |
| `ManyToManyField`       | A field for many-to-many relationships.                                                          | `to`, `related_name`, `related_query_name`, `db_table`, `symmetrical`                                                | `required=True`             | -                                                                                                        |
| `OneToOneField`         | A field for one-to-one relationships.                                                            | `to`, `on_delete`, `related_name`, `related_query_name`, `null`, `blank`, `default`                                  | `required=True`             | -                                                                                                        |
| `DecimalField`          | A field for fixed-point decimal numbers.                                                        | `max_digits`, `decimal_places`, `null`, `blank`, `default`, `unique`, `db_index`                                     | `required=True`             | `max_digits`: Total digits, `decimal_places`: Digits after decimal                                       |

---

### **Meta Options in Models**

The `Meta` class is used to define metadata for a model.

```python
class MyModel(models.Model):
    name = models.CharField(max_length=100)

    class Meta:
        db_table = "my_table_name"  # Custom database table name
        ordering = ["-name"]       # Default ordering
        verbose_name = "My Model"  # Human-readable name
```

| **Meta Option**    | **Description**                                     | **Example**                     |
|---------------------|-----------------------------------------------------|---------------------------------|
| `db_table`         | Custom database table name.                        | `db_table = "custom_name"`     |
| `ordering`         | Default ordering for queries.                      | `ordering = ["-created_at"]`   |
| `verbose_name`     | Singular human-readable name for the model.         | `verbose_name = "My Model"`    |
| `verbose_name_plural` | Plural human-readable name for the model.         | `verbose_name_plural = "Models"`|

---

### **Relationships**

1. **One-to-One**: Use `OneToOneField` to represent a one-to-one relationship.
   ```python
   class Profile(models.Model):
       user = models.OneToOneField(User, on_delete=models.CASCADE)
   ```

2. **One-to-Many**: Use `ForeignKey` to represent a one-to-many relationship.
   ```python
   class Post(models.Model):
       author = models.ForeignKey(User, on_delete=models.CASCADE)
   ```

3. **Many-to-Many**: Use `ManyToManyField` to represent a many-to-many relationship.
   ```python
   class Book(models.Model):
       authors = models.ManyToManyField(Author)
   ```

---

### **QuerySet Examples**

1. **Creating Objects**:
   ```python
   obj = MyModel.objects.create(name="John", age=25)
   ```

2. **Retrieving Objects**:
   ```python
   obj = MyModel.objects.get(id=1)
   all_objs = MyModel.objects.all()
   ```

3. **Filtering Objects**:
   ```python
   objs = MyModel.objects.filter(age__gte=18)
   ```

4. **Updating Objects**:
   ```python
   obj = MyModel.objects.get(id=1)
   obj.name = "Updated Name"
   obj.save()
   ```

5. **Deleting Objects**:
   ```python
   obj = MyModel.objects.get(id=1)
   obj.delete()
   ```
---

### **Custom Model Methods**
Django models can have custom methods to encapsulate business logic.

```python
class MyModel(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()

    def is_adult(self):
        return self.age >= 18
```

- **When to Use**: Encapsulate reusable logic that pertains specifically to a model instance.
- **Example Usage**:
  ```python
  obj = MyModel.objects.get(id=1)
  print(obj.is_adult())  # Returns True or False
  ```

---

### **Signals**
Signals allow you to hook into Django's ORM events.

```python
from django.db.models.signals import post_save
from django.dispatch import receiver

class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)

@receiver(post_save, sender=User)
def create_user_profile(sender, instance, created, **kwargs):
    if created:
        Profile.objects.create(user=instance)
```

- **Common Signals**:
  | **Signal**        | **Trigger**                       |
  |-------------------|-----------------------------------|
  | `pre_save`        | Before saving an object.          |
  | `post_save`       | After saving an object.           |
  | `pre_delete`      | Before deleting an object.        |
  | `post_delete`     | After deleting an object.         |

---

### **Inheritance in Models**
Django supports three types of model inheritance:
1. **Abstract Base Classes**: Use when you want to define common fields or methods.
   ```python
   class BaseModel(models.Model):
       created_at = models.DateTimeField(auto_now_add=True)

       class Meta:
           abstract = True

   class MyModel(BaseModel):
       name = models.CharField(max_length=100)
   ```

2. **Multi-Table Inheritance**: Each model gets its own table.
   ```python
   class BaseModel(models.Model):
       name = models.CharField(max_length=100)

   class MyModel(BaseModel):
       extra_field = models.TextField()
   ```

3. **Proxy Models**: Use to change behavior without altering fields.
   ```python
   class MyModel(BaseModel):
       class Meta:
           proxy = True
   ```

---

### **Advanced QuerySet Features**

#### Aggregations
```python
from django.db.models import Count, Avg

result = MyModel.objects.aggregate(average_age=Avg('age'))
```

#### Annotations
```python
from django.db.models import Count

result = MyModel.objects.annotate(post_count=Count('post'))
```

#### Raw SQL Queries
```python
result = MyModel.objects.raw('SELECT * FROM my_table')
```

---

### **Validation**
Custom validation can be added to models using the `clean` method or field-specific validation.

```python
from django.core.exceptions import ValidationError

class MyModel(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()

    def clean(self):
        if self.age < 0:
            raise ValidationError("Age cannot be negative.")
```

---

### **Indexing**
Indexes improve database query performance. You can define them in the `Meta` class.

```python
class MyModel(models.Model):
    name = models.CharField(max_length=100)

    class Meta:
        indexes = [
            models.Index(fields=['name']),
        ]
```

---

### **Performance Optimization**
1. **Use `select_related` and `prefetch_related`** for reducing database queries in relationships:
   ```python
   queryset = MyModel.objects.select_related('related_model')
   queryset = MyModel.objects.prefetch_related('many_to_many_field')
   ```

2. **Defer Loading** of fields:
   ```python
   queryset = MyModel.objects.defer('large_field')
   ```

3. **Database Transactions**:
   ```python
   from django.db import transaction

   with transaction.atomic():
       obj = MyModel.objects.create(name="John")
   ```

---

### **Migration Management**
Django migrations handle database schema changes.

#### Common Commands:
| **Command**                 | **Description**                                      |
|-----------------------------|----------------------------------------------------|
| `python manage.py makemigrations` | Create new migration files.                    |
| `python manage.py migrate`        | Apply migrations to the database.             |
| `python manage.py showmigrations` | Show applied and unapplied migrations.         |

#### Custom Migrations
You can write custom migration logic:
```python
from django.db import migrations

def custom_migration(apps, schema_editor):
    MyModel = apps.get_model('my_app', 'MyModel')
    MyModel.objects.create(name="Default")

class Migration(migrations.Migration):
    dependencies = []

    operations = [
        migrations.RunPython(custom_migration),
    ]
```

---

### **Admin Integration**
The Django admin site provides an interface to manage models.

#### Registering a Model:
```python
# inside admin.py
from django.contrib import admin
from .models import MyModel

@admin.register(MyModel)
class MyModelAdmin(admin.ModelAdmin):
    list_display = ('name', 'age')
```

#### Admin Features:
| **Feature**         | **Description**                                  |
|---------------------|--------------------------------------------------|
| `list_display`      | Fields to display in the admin list view.         |
| `search_fields`     | Fields to include in the search bar.              |
| `list_filter`       | Fields to filter by in the admin sidebar.         |
| `readonly_fields`   | Fields that are read-only in the admin.           |

---

### **Testing Models**
Unit tests ensure your models work as expected.

```python
from django.test import TestCase
from .models import MyModel

class MyModelTestCase(TestCase):
    def test_create_object(self):
        obj = MyModel.objects.create(name="Test", age=25)
        self.assertEqual(obj.name, "Test")
```

---

### **Cases and Usage**

| **Case**                     | **Field Types**                        | **Meta Options**             | **Notes**                            |
|-------------------------------|----------------------------------------|------------------------------|--------------------------------------|
| Blog Application              | `CharField`, `TextField`, `ForeignKey` | `ordering`, `verbose_name`   | Posts with authors and content       |
| E-Commerce                    | `CharField`, `DecimalField`, `ForeignKey`, `ManyToManyField` | `db_table`, `ordering`       | Products with categories and pricing |
| User Profiles                 | `OneToOneField`, `ImageField`, `TextField` | `verbose_name`              | Profiles linked to Django Users      |
| Inventory Management          | `CharField`, `IntegerField`, `DecimalField` | `ordering`, `db_table`       | Track stock quantities and costs     |

---

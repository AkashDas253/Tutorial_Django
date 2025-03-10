
## Django Model: Comprehensive Cheatsheet  

---

### Syntax:
  ```python
  # app/models.py
  from django.db import models  # Import models module

  class ModelName(models.Model):  # Define a new model
      field_name = models.FieldType()  # Define model fields
      # Additional fields can be added here

      class Meta:  # Optional
          key = value  # Meta options for the model
          # Additional meta options can be added here

      def function(self):  # Optional
          # Custom method for the model
          return specific_value

      def __str__(self):  # Optional but recommended
          # String representation of the object
          return string_representation  # Return string representation of object
```

---

### Model Definition  

| Concept | Syntax | Description |
|---------|--------|-------------|
| Model Class | `class ModelName(models.Model):` | Defines a model as a table. |
| Field | `field_name = models.FieldType(options)` | Defines attributes as table columns. |
| Meta Options | `class Meta:` | Defines metadata for table behavior. |
| String Representation | `def __str__(self):` | Returns a human-readable model name. |

### Common Field Types  

| Field Type | Syntax | Description |
|------------|--------|-------------|
| `CharField` | `models.CharField(max_length=255)` | Stores short text. |
| `TextField` | `models.TextField()` | Stores long text. |
| `IntegerField` | `models.IntegerField()` | Stores integers. |
| `FloatField` | `models.FloatField()` | Stores floating-point numbers. |
| `BooleanField` | `models.BooleanField()` | Stores `True/False`. |
| `DateField` | `models.DateField(auto_now_add=True)` | Stores dates. |
| `DateTimeField` | `models.DateTimeField(auto_now=True)` | Stores timestamps. |
| `ForeignKey` | `models.ForeignKey(Model, on_delete=models.CASCADE)` | Defines a many-to-one relationship. |
| `ManyToManyField` | `models.ManyToManyField(Model)` | Defines a many-to-many relationship. |
| `OneToOneField` | `models.OneToOneField(Model, on_delete=models.CASCADE)` | Defines a one-to-one relationship. |

### Field Options  

| Option | Usage | Description |
|--------|-------|-------------|
| `null=True` | `models.CharField(null=True)` | Allows `NULL` values in the database. |
| `blank=True` | `models.CharField(blank=True)` | Allows empty input in forms. |
| `default=value` | `models.IntegerField(default=0)` | Sets a default value. |
| `unique=True` | `models.EmailField(unique=True)` | Ensures unique values. |
| `choices=` | `models.CharField(choices=[(1, "A"), (2, "B")])` | Defines dropdown choices. |
| `db_index=True` | `models.IntegerField(db_index=True)` | Adds a database index. |

### Model Meta Options  

| Option | Usage | Description |
|--------|-------|-------------|
| `db_table` | `db_table = "custom_table_name"` | Custom table name. |
| `ordering` | `ordering = ["-created_at"]` | Default ordering. |
| `verbose_name` | `verbose_name = "Model Name"` | Singular display name. |
| `verbose_name_plural` | `verbose_name_plural = "Models"` | Plural display name. |

### Querying Models  

| Operation | Syntax |
|-----------|--------|
| Create | `Model.objects.create(field=value)` |
| Retrieve All | `Model.objects.all()` |
| Retrieve One | `Model.objects.get(id=1)` |
| Filter | `Model.objects.filter(field=value)` |
| Exclude | `Model.objects.exclude(field=value)` |
| Order By | `Model.objects.order_by("field")` |
| Update | `Model.objects.filter(id=1).update(field=value)` |
| Delete | `Model.objects.get(id=1).delete()` |

### Model Inheritance  

| Type | Syntax | Description |
|------|--------|-------------|
| Abstract Base Class | `class Base(models.Model): class Meta: abstract = True` | Defines reusable fields. |
| Multi-Table | `class Child(Base):` | Creates separate tables for parent-child. |
| Proxy Model | `class Proxy(Model): class Meta: proxy = True` | Modifies behavior without changing structure. |

### Migrations  

| Command | Syntax |
|---------|--------|
| Create Migration | `python manage.py makemigrations` |
| Apply Migration | `python manage.py migrate` |
| Show Migrations | `python manage.py showmigrations` |

### Model Signals  

| Signal | Usage |
|--------|-------|
| `pre_save` | `@receiver(pre_save, sender=Model)` |
| `post_save` | `@receiver(post_save, sender=Model)` |
| `pre_delete` | `@receiver(pre_delete, sender=Model)` |
| `post_delete` | `@receiver(post_delete, sender=Model)` |

### Transactions  

| Function | Usage |
|----------|-------|
| Atomic Transaction | `with transaction.atomic():` |
| Row Locking | `Model.objects.select_for_update()` |

## Model Definition

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

### Common Field Types
- `CharField(max_length=100, **options)`  # String field with max length
  - `max_length`: Maximum length of the string
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
  - `unique`: Ensure unique values
  - `choices`: Define choices for the field
- `TextField(**options)`  # Large text field
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
- `IntegerField(**options)`  # Integer field
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
  - `unique`: Ensure unique values
- `FloatField(**options)`  # Float field
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
  - `unique`: Ensure unique values
- `BooleanField(**options)`  # Boolean field
  - `null`: Allow NULL values
  - `default`: Set default value
- `DateField(**options)`  # Date field
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
- `DateTimeField(**options)`  # Date and time field
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
- `EmailField(**options)`  # Email field
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
  - `unique`: Ensure unique values
- `URLField(**options)`  # URL field
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
  - `unique`: Ensure unique values
- `FileField(upload_to='uploads/', **options)`  # File upload field
  - `upload_to`: Directory to upload files
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
- `ImageField(upload_to='images/', **options)`  # Image upload field
  - `upload_to`: Directory to upload images
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value

### Field Options
- `null=True`  # Allow NULL values
- `blank=True`  # Allow blank values
- `default=value`  # Set default value
- `unique=True`  # Ensure unique values
- `choices=[(value1, 'Label1'), (value2, 'Label2')]`  # Define choices for the field

### Relation Fields
- `AutoField(primary_key=True, **options)`  # Define a primary key (automatically added by Django)
  - `primary_key`: Set as primary key
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
- `ForeignKey('OtherModel', on_delete=models.CASCADE, **options)`  # Define a foreign key relationship
  - `on_delete`: Behavior when the referenced object is deleted
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
- `OneToOneField('OtherModel', on_delete=models.CASCADE, **options)`  # Define a one-to-one relationship
  - `on_delete`: Behavior when the referenced object is deleted
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value
- `ManyToManyField('OtherModel', **options)`  # Define a many-to-many relationship
  - `null`: Allow NULL values
  - `blank`: Allow blank values
  - `default`: Set default value

### Model Methods
- `save(force_insert=False, force_update=False, using=None, update_fields=None)`  # Save the current instance
  - `force_insert`: Force an SQL INSERT
  - `force_update`: Force an SQL UPDATE
  - `using`: Database alias to use
  - `update_fields`: Fields to update
- `delete(using=None, keep_parents=False)`  # Delete the current instance
  - `using`: Database alias to use
  - `keep_parents`: Keep parent relationships
- `get_absolute_url()`  # Return the absolute URL for the instance
- `clean()`  # Validate the model instance
- `__str__()`  # Return a string representation of the instance

### Meta Options
- `ordering = ['field_name']`  # Default ordering
  - `ordering`: List of fields to order by
- `verbose_name = "Model Name"`  # Human-readable name for the model
  - `verbose_name`: Singular name for the model
- `verbose_name_plural = "Model Names"`  # Human-readable plural name for the model
  - `verbose_name_plural`: Plural name for the model
- `db_table = 'table_name'`  # Custom database table name
  - `db_table`: Name of the database table
- `unique_together = (('field1', 'field2'),)`  # Unique constraint across multiple fields
  - `unique_together`: Fields that must be unique together
- `index_together = (('field1', 'field2'),)`  # Index constraint across multiple fields
  - `index_together`: Fields that should be indexed together



### **Django Models: Comprehensive Note**

Django **models** act as the interface to your database, enabling you to define data structure and interact with it using Python code. A model is a Python class derived from `django.db.models.Model`, and each attribute of the class represents a database field.

---



---

### **Common Model Fields and Their Parameters**

| **Field Type**         | **Description**                                                                                     | **Parameters**                                                                                      | **Default Value**      | **Range/Options**                                                                                          |
|-------------------------|----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|------------------------|------------------------------------------------------------------------------------------------------------|
| **`CharField`**         | Stores character strings with a fixed maximum length.                                              | `max_length`, `null`, `blank`, `unique`, `db_index`, `default`                                      | `null=False`, `blank=False` | `max_length`: Required Integer                                                                               |
| **`TextField`**         | For large text fields, e.g., descriptions or comments.                                             | `null`, `blank`, `db_index`, `default`                                                             | `null=False`, `blank=False` | No limit on text size                                                                                        |
| **`IntegerField`**      | Stores integer values.                                                                             | `null`, `blank`, `default`, `validators`                                                           | `null=False`, `blank=False` | Integer values                                                                                              |
| **`FloatField`**        | Stores floating-point numbers.                                                                     | `null`, `blank`, `default`, `validators`                                                           | `null=False`, `blank=False` | Floating-point numbers                                                                                       |
| **`BooleanField`**      | Stores `True` or `False`.                                                                          | `default`, `db_index`                                                                               | `default=False`        | `True` or `False`                                                                                           |
| **`DateField`**         | Stores date values.                                                                                | `null`, `blank`, `auto_now`, `auto_now_add`, `default`                                              | `null=False`, `blank=False` | Valid date format (e.g., `YYYY-MM-DD`)                                                                      |
| **`DateTimeField`**     | Stores date and time values.                                                                       | `null`, `blank`, `auto_now`, `auto_now_add`, `default`                                              | `null=False`, `blank=False` | Valid date-time format                                                                                       |
| **`EmailField`**        | Ensures the input matches a valid email format.                                                    | `max_length`, `null`, `blank`, `unique`, `db_index`, `default`                                      | `null=False`, `blank=False` | `max_length`: Default 254                                                                                   |
| **`URLField`**          | Stores a valid URL.                                                                                | `max_length`, `null`, `blank`, `unique`, `db_index`, `default`                                      | `null=False`, `blank=False` | `max_length`: Default 200                                                                                   |
| **`FileField`**         | Handles file uploads by storing the file path.                                                    | `upload_to`, `max_length`, `null`, `blank`, `default`                                               | `null=False`, `blank=False` | Valid file path                                                                                             |
| **`ImageField`**        | Subclass of `FileField` specifically for images.                                                   | `upload_to`, `max_length`, `null`, `blank`, `default`                                               | `null=False`, `blank=False` | Valid image formats                                                                                         |
| **`ForeignKey`**        | Defines a many-to-one relationship with another model.                                             | `to`, `on_delete`, `related_name`, `related_query_name`, `null`, `blank`, `db_index`, `default`     | `null=False`, `on_delete=CASCADE` | `to`: Related model                                                                                        |
| **`OneToOneField`**     | Defines a one-to-one relationship with another model.                                              | `to`, `on_delete`, `related_name`, `related_query_name`, `null`, `blank`, `db_index`, `default`     | `null=False`, `on_delete=CASCADE` | Unique relationship                                                                                         |
| **`ManyToManyField`**   | Defines a many-to-many relationship with another model.                                            | `to`, `related_name`, `related_query_name`, `blank`, `through`                                      | `blank=False`          | `to`: Related model                                                                                        |
| **`SlugField`**         | Stores a short, label-friendly string (used in URLs).                                              | `max_length`, `allow_unicode`, `db_index`, `unique`, `null`, `blank`, `default`                     | `null=False`, `blank=False` | `max_length`: Default 50                                                                                    |
| **`JSONField`**         | Stores JSON-encoded data.                                                                          | `null`, `blank`, `default`, `db_index`                                                             | `null=False`, `blank=False` | Valid JSON                                                                                                  |

---

### **Common Field Parameters**

1. **`null`**: Determines if the database column allows `NULL` values (`null=True`).
2. **`blank`**: Determines if the field is optional in forms (`blank=True`).
3. **`default`**: Provides a default value for the field.
4. **`unique`**: Ensures the field’s value is unique in the table.
5. **`db_index`**: Creates a database index for faster lookups.
6. **`choices`**: Defines a fixed set of valid options for the field.
7. **`validators`**: Adds custom validation logic.
8. **`related_name`**: Sets the name of the reverse relationship for `ForeignKey` or `ManyToManyField`.

---

### **Model Definition Example**

```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100, unique=True)
    description = models.TextField(blank=True, null=True)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    available = models.BooleanField(default=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.name
```

---

### **Field Relationships**

#### **ForeignKey Example**
```python
class Author(models.Model):
    name = models.CharField(max_length=50)

class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.ForeignKey(Author, on_delete=models.CASCADE, related_name="books")
```

#### **OneToOneField Example**
```python
class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    bio = models.TextField(blank=True)
```

#### **ManyToManyField Example**
```python
class Student(models.Model):
    name = models.CharField(max_length=50)

class Course(models.Model):
    title = models.CharField(max_length=100)
    students = models.ManyToManyField(Student, related_name="courses")
```

---

### **Meta Class in Models**
The `Meta` class provides metadata options for a model.

```python
class Product(models.Model):
    name = models.CharField(max_length=100)

    class Meta:
        ordering = ['name']
        verbose_name = "Product"
        verbose_name_plural = "Products"
```

---

This note provides a detailed overview of Django models, including field types, parameters, relationships, and examples. Let me know if you’d like to dive deeper into advanced features or specific use cases!
## Model Definition

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

### Relation Fields

| **Field Type**       | **Description**                       | **Syntax Example**                                         | **Parameters with default value and description with type**                                                              | **Required Parameter**       |
|----------------------|---------------------------------------|------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|------------------------------|
| `AutoField`          | Define a primary key (automatically added by Django) | `models.AutoField(primary_key=True, **options)`            | `primary_key=True`: Set as primary key (bool), `null=False`: Allow NULL values (bool), `blank=False`: Allow blank values (bool), `default=None`: Set default value (any) | `primary_key`                |
| `ForeignKey`         | Define a foreign key relationship     | `models.ForeignKey('OtherModel', on_delete=models.CASCADE, **options)` | `on_delete=models.CASCADE`: Behavior when the referenced object is deleted (function), `null=False`: Allow NULL values (bool), `blank=False`: Allow blank values (bool), `default=None`: Set default value (any) | `on_delete`                  |
| `OneToOneField`      | Define a one-to-one relationship      | `models.OneToOneField('OtherModel', on_delete=models.CASCADE, **options)` | `on_delete=models.CASCADE`: Behavior when the referenced object is deleted (function), `null=False`: Allow NULL values (bool), `blank=False`: Allow blank values (bool), `default=None`: Set default value (any) | `on_delete`                  |
| `ManyToManyField`    | Define a many-to-many relationship    | `models.ManyToManyField('OtherModel', **options)`          | `null=False`: Allow NULL values (bool), `blank=False`: Allow blank values (bool), `default=None`: Set default value (any) | None                         |

---

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

---

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

---
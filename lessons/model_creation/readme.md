
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

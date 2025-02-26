## Model Methods in Django

Model methods in Django are used to define custom behaviors and functionalities for your model instances. These methods can be categorized into instance methods, class methods, static methods, custom model managers, and model properties. Here is the syntax and usage for each type, including parameter lists:

#### **Instance Methods**

Instance methods operate on individual instances of the model.

```python
class MyModel(models.Model):
    field_name = models.CharField(max_length=100)

    def instance_method(self, param1, param2):
        # Perform actions or calculations specific to this instance
        return f"Instance method called with {param1} and {param2}"
```

- `self`: Reference to the current instance of the model.
- `param1`, `param2`: Example parameters for the method.

**Usage:**
```python
instance = MyModel(field_name="example")
result = instance.instance_method("value1", "value2")
print(result)  # Output: Instance method called with value1 and value2
```

#### **Class Methods**

Class methods operate on the model class itself and are defined using the `@classmethod` decorator.

```python
class MyModel(models.Model):
    field_name = models.CharField(max_length=100)

    @classmethod
    def class_method(cls, param1, param2):
        # Perform actions related to the model class
        return f"Class method called with {param1} and {param2}"
```

- `cls`: Reference to the model class.
- `param1`, `param2`: Example parameters for the method.

**Usage:**
```python
result = MyModel.class_method("value1", "value2")
print(result)  # Output: Class method called with value1 and value2
```

#### **Static Methods**

Static methods are utility functions related to the model and are defined using the `@staticmethod` decorator.

```python
class MyModel(models.Model):
    field_name = models.CharField(max_length=100)

    @staticmethod
    def static_method(param1, param2):
        # Perform utility actions
        return f"Static method called with {param1} and {param2}"
```

- `param1`, `param2`: Example parameters for the method.

**Usage:**
```python
result = MyModel.static_method("value1", "value2")
print(result)  # Output: Static method called with value1 and value2
```

#### **Custom Model Managers**

Custom model managers add extra manager methods to your models by subclassing `models.Manager`.

```python
class MyModelManager(models.Manager):
    def custom_manager_method(self, param1, param2):
        # Custom manager method
        return f"Manager method called with {param1} and {param2}"

class MyModel(models.Model):
    field_name = models.CharField(max_length=100)

    objects = MyModelManager()
```

- `self`: Reference to the manager instance.
- `param1`, `param2`: Example parameters for the method.

**Usage:**
```python
result = MyModel.objects.custom_manager_method("value1", "value2")
print(result)  # Output: Manager method called with value1 and value2
```

#### **Model Properties**

Model properties define read-only attributes calculated from other fields in the model using the `@property` decorator.

```python
class MyModel(models.Model):
    field_name = models.CharField(max_length=100)

    @property
    def calculated_property(self):
        # Calculate and return a value based on other fields
        return f"Calculated property based on {self.field_name}"
```

- No parameters are needed for properties.

**Usage:**
```python
instance = MyModel(field_name="example")
print(instance.calculated_property)  # Output: Calculated property based on example
```

### Summary of Syntax

- **Instance Methods**: Define methods that operate on individual instances.
  - Parameters: `self`, `param1`, `param2`, ...
- **Class Methods**: Use `@classmethod` to define methods that operate on the model class.
  - Parameters: `cls`, `param1`, `param2`, ...
- **Static Methods**: Use `@staticmethod` to define utility functions related to the model.
  - Parameters: `param1`, `param2`, ...
- **Custom Model Managers**: Subclass `models.Manager` to add custom manager methods.
  - Parameters: `self`, `param1`, `param2`, ...
- **Model Properties**: Use `@property` to define read-only attributes calculated from other fields.
  - No parameters needed.

These syntaxes provide a structured way to enhance the functionality of Django models.
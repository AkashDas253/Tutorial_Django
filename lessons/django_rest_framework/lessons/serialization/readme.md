## Serialization in Django REST Framework

### Purpose

Serialization is the process of converting complex data types like Django models or Python objects into native Python data types (dict, list) that can be easily rendered into JSON, XML, or other content types. Deserialization is the reverse: converting parsed data back into complex types and validating incoming data.

---

### Key Classes

#### Serializer

* Inherits from `rest_framework.serializers.Serializer`
* Works like a Django `Form`
* Explicitly defines each field
* Suitable for full control over how data is serialized/deserialized

```python
from rest_framework import serializers

class ExampleSerializer(serializers.Serializer):
    name = serializers.CharField(max_length=100)
    age = serializers.IntegerField()
```

#### ModelSerializer

* Inherits from `serializers.ModelSerializer`
* Automatically creates fields based on Django model
* Reduces boilerplate

```python
class ExampleModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = ExampleModel
        fields = ['name', 'age']
```

#### HyperlinkedModelSerializer

* Like `ModelSerializer` but uses hyperlinks for relationships
* Requires `HyperlinkedIdentityField` and view names

---

### Core Components

#### Fields

Common serializer fields:

* CharField, IntegerField, FloatField, BooleanField
* DateField, DateTimeField
* EmailField, URLField, UUIDField
* FileField, ImageField
* SlugRelatedField, PrimaryKeyRelatedField, HyperlinkedRelatedField

#### ReadOnlyField and WriteOnlyField

* `read_only=True`: Field is only used for serialization
* `write_only=True`: Field is only used for deserialization

---

### Validation

#### Field-level Validation

```python
def validate_age(self, value):
    if value < 0:
        raise serializers.ValidationError("Age must be positive.")
    return value
```

#### Object-level Validation

```python
def validate(self, attrs):
    if attrs['start'] > attrs['end']:
        raise serializers.ValidationError("Start must be before end.")
    return attrs
```

---

### Custom Fields

Define a field with custom logic for serialization and deserialization:

```python
class CustomField(serializers.Field):
    def to_representation(self, value):
        return value.custom_format()

    def to_internal_value(self, data):
        return parse_custom_format(data)
```

---

### SerializerMethodField

Used to include computed or read-only data:

```python
from rest_framework import serializers

class ExampleSerializer(serializers.ModelSerializer):
    full_name = serializers.SerializerMethodField()

    def get_full_name(self, obj):
        return f"{obj.first_name} {obj.last_name}"

    class Meta:
        model = ExampleModel
        fields = ['id', 'full_name']
```

---

### Nested Serializers

Used to serialize related objects:

```python
class ProfileSerializer(serializers.ModelSerializer):
    class Meta:
        model = Profile
        fields = ['bio']

class UserSerializer(serializers.ModelSerializer):
    profile = ProfileSerializer()

    class Meta:
        model = User
        fields = ['username', 'profile']
```

---

### Partial Updates

Handled by setting `partial=True` when initializing the serializer:

```python
serializer = ExampleSerializer(instance, data=request.data, partial=True)
```

---

### Meta Options in ModelSerializer

* `model`: Target Django model
* `fields`: List of fields or `'__all__'`
* `exclude`: Fields to exclude
* `read_only_fields`: Fields that cannot be written
* `depth`: Level of related serialization (not preferred for deep nesting)

---

### Inheritance and Composition

* Serializers can inherit and extend other serializers
* Useful for DRY and modular designs

---

### Performance Considerations

* Prefer `ModelSerializer` when possible for cleaner, optimized code
* Minimize nesting when unnecessary
* Use `select_related` and `prefetch_related` in views to reduce queries

---

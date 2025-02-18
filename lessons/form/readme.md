## **Django Forms Cheatsheet**  

#### **Types of Forms**  
- **`forms.Form`** – Manual form creation (not linked to a model).  
- **`forms.ModelForm`** – Auto-generates form fields from a model.  

#### **Creating a Basic Form**  
```python
from django import forms

class MyForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    age = forms.IntegerField()
```

#### **Form Field Types**  
| Field Type | Description |
|------------|------------|
| `CharField(max_length=n)` | Text input. |
| `EmailField()` | Validates email format. |
| `IntegerField()` | Accepts only integers. |
| `FloatField()` | Accepts decimal numbers. |
| `BooleanField()` | Checkbox input. |
| `DateField()` | Accepts a date. |
| `DateTimeField()` | Accepts a date and time. |
| `ChoiceField(choices=[(key, value)])` | Dropdown/select field. |
| `MultipleChoiceField(choices=[(key, value)])` | Multi-select field. |

#### **Handling Forms in Views**  
```python
from django.shortcuts import render
from .forms import MyForm

def my_view(request):
    if request.method == "POST":
        form = MyForm(request.POST)
        if form.is_valid():
            name = form.cleaned_data['name']
    else:
        form = MyForm()
    
    return render(request, 'template.html', {'form': form})
```

#### **Rendering Forms in Templates**  
```html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Submit</button>
</form>
```

#### **Model Forms**  
- Auto-generates form fields from a model.  

```python
from django import forms
from .models import MyModel

class MyModelForm(forms.ModelForm):
    class Meta:
        model = MyModel
        fields = ['name', 'age']
```

#### **Form Validation**  
- Custom validation using `clean_<fieldname>()`.  

```python
class MyForm(forms.Form):
    age = forms.IntegerField()

    def clean_age(self):
        age = self.cleaned_data.get('age')
        if age < 18:
            raise forms.ValidationError("Must be 18 or older.")
        return age
```

#### **Widgets (Customizing Input Fields)**  
```python
class CustomForm(forms.Form):
    name = forms.CharField(widget=forms.TextInput(attrs={'class': 'custom-class'}))
```

| Widget | Description |
|--------|------------|
| `TextInput` | Standard text input. |
| `Textarea` | Multi-line text input. |
| `CheckboxInput` | Checkbox input. |
| `Select` | Dropdown menu. |
| `RadioSelect` | Radio buttons. |
| `FileInput` | File upload input. |

#### **CSRF Protection**  
- Always include `{% csrf_token %}` in forms.  

---

## Forms in Django

Django provides a robust and easy-to-use framework for managing forms. Forms in Django are used for gathering input from users, validating the input, and handling the data.

---

### **What are Forms in Django?**
Forms in Django allow you to define fields for user input, validate the input, and render HTML automatically. There are two main types:
1. **Django Forms** (`django.forms.Form`): Used for manual form handling.
2. **Model Forms** (`django.forms.ModelForm`): Automatically tied to a Django model.

### Key Concepts of Django Forms

- **Forms**: Represents HTML forms in Python.
- **Form Fields**: Represent individual input fields, providing validation and rendering.
- **Validation**: Django provides built-in mechanisms for validating data.
- **Form Handling**: Processing both GET and POST data efficiently.

---

### **Syntax for Creating Forms**
Forms in Django can be created in two ways:

1. **Using `forms.Form` (Functional Approach)**:
   ```python
   from django import forms

   class MyForm(forms.Form):
       name = forms.CharField(max_length=100, required=True)
       age = forms.IntegerField(min_value=1, max_value=120)
       email = forms.EmailField(required=True)
   ```

2. **Using `forms.ModelForm` (Model-Based Approach)**:
   ```python
   from django import forms
   from myapp.models import MyModel

   class MyModelForm(forms.ModelForm):
       class Meta:
           model = MyModel
           fields = ['name', 'email']
   ```

---

### **Key Parameters of Form Fields**
The table below outlines common parameters used across different field types.

| **Parameter**     | **Description**                                                                 | **Range/Options**                       | **Default Value** |
|--------------------|---------------------------------------------------------------------------------|-----------------------------------------|-------------------|
| `required`         | Specifies if the field is mandatory.                                           | `True`, `False`                         | `True`            |
| `label`            | Custom label for the field.                                                   | Any string                              | Field name        |
| `initial`          | Initial value for the field.                                                  | Any valid input                         | `None`            |
| `help_text`        | Additional information displayed with the field.                              | Any string                              | `None`            |
| `max_length`       | Maximum length for text-based fields.                                          | Positive integer                        | `None`            |
| `min_length`       | Minimum length for text-based fields.                                          | Positive integer                        | `None`            |
| `max_value`        | Maximum value for numeric fields.                                             | Number                                  | `None`            |
| `min_value`        | Minimum value for numeric fields.                                             | Number                                  | `None`            |
| `validators`       | List of custom validators for the field.                                      | Callable functions                      | `None`            |
| `widget`           | Widget class to customize the field’s rendering.                              | Widget instance                         | Default widget    |
| `error_messages`   | Custom error messages for validation.                                         | Dictionary of error messages            | Default messages  |

### **Common Form Fields and Their Parameters**
| **Field Type**         | **Description**                       | **Syntax Example**                                         | **Parameters**                                                              | **Default Value**   | **Range/Options**                                                                                         |
|------------------------|---------------------------------------|------------------------------------------------------------|----------------------------------------------------------------------------|---------------------|-----------------------------------------------------------------------------------------------------------|
| `CharField`            | Text input                            | `forms.CharField(max_length=100, required=True)`           | `max_length`, `min_length`, `required`, `label`, `widget`, `initial`        | `required=True`     | `max_length`: Integer, `min_length`: Integer                                                             |
| `EmailField`           | Email input                           | `forms.EmailField(required=True)`                          | `required`, `label`, `widget`, `initial`                                   | `required=True`     | Valid email format                                                                                       |
| `IntegerField`         | Numeric input                         | `forms.IntegerField(min_value=1, max_value=100)`           | `min_value`, `max_value`, `required`, `label`, `widget`, `initial`          | `required=True`     | `min_value`: Integer, `max_value`: Integer                                                               |
| `DateField`            | Date input                            | `forms.DateField(widget=forms.SelectDateWidget)`           | `required`, `label`, `widget`, `input_formats`, `initial`                  | `required=True`     | `input_formats`: List of date formats                                                                   |
| `BooleanField`         | Checkbox for boolean values           | `forms.BooleanField(required=False)`                       | `required`, `label`, `widget`, `initial`                                   | `required=False`    | `initial`: `True` or `False`                                                                            |
| `ChoiceField`          | Dropdown/select input                 | `forms.ChoiceField(choices=[('1', 'One'), ('2', 'Two')])`  | `choices`, `required`, `label`, `widget`, `initial`                        | `required=True`     | `choices`: List of tuples (e.g., `[('A', 'Option A'), ('B', 'Option B')]`)                              |
| `MultipleChoiceField`  | Multiple select input                 | `forms.MultipleChoiceField(choices=[('1', 'One'), ('2', 'Two')])` | `choices`, `required`, `label`, `widget`, `initial`                        | `required=True`     | `choices`: List of tuples                                                                               |
| `FileField`            | File upload                           | `forms.FileField(required=True)`                           | `required`, `label`, `widget`, `initial`, `max_length`                     | `required=True`     | `max_length`: Integer                                                                                   |
| `ImageField`           | Image upload (inherits `FileField`)   | `forms.ImageField(required=True)`                          | `required`, `label`, `widget`, `initial`, `max_length`                     | `required=True`     | Image file formats                                                                                      |
| `URLField`             | URL input                             | `forms.URLField(required=True)`                            | `required`, `label`, `widget`, `initial`                                   | `required=True`     | Valid URL format                                                                                        |

---

### **Customizing Widgets**
Widgets control the rendering of form fields. Here’s an example of using widgets:
```python
from django import forms

class MyCustomForm(forms.Form):
    name = forms.CharField(widget=forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'Enter your name'}))
    password = forms.CharField(widget=forms.PasswordInput(attrs={'class': 'form-control'}))
```

### **Form Widget Options**

Widgets define how the input fields are rendered in HTML.

| **Widget**               | **Use Case**                    | **Attributes**                       |
|--------------------------|----------------------------------|---------------------------------------|
| `TextInput`              | For single-line text input      | `attrs={'placeholder': 'Enter text'}`|
| `Textarea`               | For multi-line text input       | `attrs={'rows': 4, 'cols': 50}`      |
| `EmailInput`             | For email input fields          | `attrs={'placeholder': 'Email'}`     |
| `NumberInput`            | For numeric input               | `attrs={'min': 0, 'max': 100}`       |
| `DateInput`              | For date input fields           | `attrs={'type': 'date'}`             |
| `CheckboxInput`          | For boolean input               | -                                     |
| `Select`                 | For dropdowns                   | `attrs={'class': 'dropdown'}`        |
| `FileInput`              | For file uploads                | -                                     |

---

### **Custom Validation**

#### **Form Validation**
Custom validation is done by overriding the `clean` or `clean_<fieldname>` methods.

#### Using `clean()` Method:
```python
class MyForm(forms.Form):
    name = forms.CharField(max_length=100)

    def clean_name(self):
        name = self.cleaned_data.get('name')
        if 'admin' in name:
            raise forms.ValidationError("Invalid name!")
        return name
```

#### Using `validators`:
```python
from django.core.validators import RegexValidator

class MyForm(forms.Form):
    username = forms.CharField(
        validators=[RegexValidator(regex=r'^[a-zA-Z0-9]*$', message='Invalid username')]
    )
```

---

### **Form Handling in Views**

#### Example of GET and POST Handling:
```python
from django.shortcuts import render
from .forms import MyForm

def form_view(request):
    if request.method == 'POST':
        form = MyForm(request.POST)
        if form.is_valid():
            # Process form data
            return render(request, 'success.html')
    else:
        form = MyForm()

    return render(request, 'form.html', {'form': form})
```

---

### **Advanced Features**

| **Feature**                  | **Description**                                                                                      | **Example**                                                                 |
|------------------------------|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Custom Error Messages        | Override default error messages.                                                                   | `forms.CharField(error_messages={'required': 'Name is required!'})`        |
| Form Sets                    | Grouping multiple forms.                                                                           | Use `formset_factory()`                                                    |
| Dynamic Forms                | Adding fields dynamically.                                                                         | Add fields in `__init__()`                                                 |
| File Handling                | Use `request.FILES` to handle uploaded files.                                                     | `form = MyForm(request.POST, request.FILES)`                               |

---

### **Cases and Usage**


| **Use Case**                   | **Solution**                                                                                           |
|--------------------------------|-------------------------------------------------------------------------------------------------------|
| Simple User Input Form         | Use `forms.Form` with fields like `CharField`, `EmailField`.                                           |
| Model-Backed Forms             | Use `ModelForm` for forms tied to database models.                                                    |
| Custom Form Validation         | Override `clean()` or `clean_<field>()`.                                                              |
| File Uploads                   | Use `FileField` or `ImageField` and process via `request.FILES`.                                      |
| Complex Field Rendering        | Customize field rendering using widgets like `Select`, `Textarea`, etc.                              |
| Dynamic Forms                  | Modify fields dynamically in the `__init__` method.                                                  |


| **Case**                            | **Form Type**      | **Fields**                   | **Validation**               |
|-------------------------------------|--------------------|------------------------------|------------------------------|
| Registration Form                   | `forms.Form`       | `username`, `email`, `password` | Validate email, password length |
| Profile Update                      | `forms.ModelForm`  | `profile_pic`, `bio`         | Validate image format        |
| Survey with Multiple Options        | `forms.Form`       | `choices` (MultipleChoice)   | Validate selection           |
| File Upload                         | `forms.Form`       | `file` (FileField)           | Validate file size and type  |
| Search Bar                          | `forms.Form`       | `query` (CharField)          | Validate query length        |

---

## **Forms in Django**

Django provides a powerful form-handling system that simplifies rendering HTML forms, validating input, and converting data to Python types. Forms can be used with or without models.

---

### **1. Types of Forms**

| Type        | Use Case                                |
| ----------- | --------------------------------------- |
| `Form`      | Manual form creation for arbitrary data |
| `ModelForm` | Auto-generate form based on model       |

---

### **2. Basic Form Example**

```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(widget=forms.Textarea)
```

---

### **3. Rendering Forms in Templates**

```html
<form method="post">
  {% csrf_token %}
  {{ form.as_p }}
  <button type="submit">Send</button>
</form>
```

Alternate renderings:

* `{{ form.as_table }}`
* `{{ form.as_ul }}`

---

### **4. Handling Form in Views**

```python
def contact_view(request):
    if request.method == 'POST':
        form = ContactForm(request.POST)
        if form.is_valid():
            # Access cleaned data
            name = form.cleaned_data['name']
            ...
    else:
        form = ContactForm()
    return render(request, 'contact.html', {'form': form})
```

---

### **5. Form Field Types**

| Field                         | Purpose            |
| ----------------------------- | ------------------ |
| `CharField`                   | Short text input   |
| `EmailField`                  | Validates email    |
| `IntegerField`                | Whole number input |
| `BooleanField`                | Checkbox           |
| `ChoiceField`                 | Dropdown           |
| `DateField` / `DateTimeField` | Dates              |
| `FileField` / `ImageField`    | File upload        |

---

### **6. Widgets**

Widgets define how form fields are rendered.

```python
forms.CharField(widget=forms.TextInput(attrs={'class': 'form-control'}))
```

Common widgets:

* `TextInput`
* `Textarea`
* `Select`
* `CheckboxInput`
* `DateInput`
* `FileInput`

---

### **7. Validation**

#### Built-in:

* Performed by default (e.g., `required`, `max_length`)

#### Custom Field Validation:

```python
def clean_name(self):
    data = self.cleaned_data['name']
    if "test" in data.lower():
        raise forms.ValidationError("Invalid name")
    return data
```

#### Whole Form Validation:

```python
def clean(self):
    cleaned_data = super().clean()
    ...
```

---

### **8. ModelForm**

Used to create/update model instances via forms.

```python
from django.forms import ModelForm
from .models import Product

class ProductForm(ModelForm):
    class Meta:
        model = Product
        fields = ['name', 'price', 'in_stock']
```

Usage in views:

```python
def add_product(request):
    form = ProductForm(request.POST or None)
    if form.is_valid():
        form.save()
    return render(request, 'add_product.html', {'form': form})
```

---

### **9. Formsets**

Formsets are collections of forms for handling multiple objects.

```python
from django.forms import formset_factory

ContactFormSet = formset_factory(ContactForm, extra=2)
```

---

### **10. Crispy Forms (Optional)**

Enhance rendering using `django-crispy-forms`.

Install:

```bash
pip install django-crispy-forms
```

In template:

```html
{% load crispy_forms_tags %}
<form method="post">
  {% csrf_token %}
  {{ form|crispy }}
</form>
```

---

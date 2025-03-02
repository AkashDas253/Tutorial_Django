### **Django Forms Cheatsheet**  

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

## **Basic Forms (`forms.Form`) in Django**  

### **Overview**  
Django's `forms.Form` class provides a structured way to handle form input, validation, and rendering. It defines fields, handles user input, and validates data before processing.  

### **Key Features**  
- Defines form fields explicitly.  
- Provides built-in validation.  
- Allows custom validation methods.  
- Supports initial values and default widgets.  

### **Defining a Basic Form**  
A basic Django form is defined as a Python class inheriting from `forms.Form`:  

```python
from django import forms  

class ContactForm(forms.Form):  
    name = forms.CharField(max_length=100)  
    email = forms.EmailField()  
    message = forms.CharField(widget=forms.Textarea)  
```

### **Form Fields**  
Each form field corresponds to a Django field type, which automatically provides validation and rendering:  

| Field Type | Description | Example |  
|------------|------------|---------|  
| `CharField` | Text input with length constraints. | `forms.CharField(max_length=100)` |  
| `EmailField` | Validates email addresses. | `forms.EmailField()` |  
| `IntegerField` | Accepts integers. | `forms.IntegerField()` |  
| `BooleanField` | Checkbox for boolean values. | `forms.BooleanField()` |  
| `ChoiceField` | Dropdown with predefined choices. | `forms.ChoiceField(choices=[('1', 'Yes'), ('0', 'No')])` |  
| `MultipleChoiceField` | Allows selecting multiple choices. | `forms.MultipleChoiceField(choices=[('1', 'Option A'), ('2', 'Option B')])` |  
| `DateField` | Accepts date input. | `forms.DateField(widget=forms.SelectDateWidget)` |  
| `FileField` | Handles file uploads. | `forms.FileField()` |  

### **Widgets**  
Widgets define how fields are rendered in HTML. Django provides default widgets, but they can be customized:  

| Widget | Description | Example |  
|--------|------------|---------|  
| `TextInput` | Standard text box. | `forms.CharField(widget=forms.TextInput(attrs={'placeholder': 'Enter name'}))` |  
| `Textarea` | Multi-line text area. | `forms.CharField(widget=forms.Textarea)` |  
| `CheckboxInput` | Checkbox for boolean values. | `forms.BooleanField(widget=forms.CheckboxInput)` |  
| `Select` | Dropdown selection. | `forms.ChoiceField(widget=forms.Select, choices=[...])` |  
| `RadioSelect` | Radio button selection. | `forms.ChoiceField(widget=forms.RadioSelect, choices=[...])` |  

### **Rendering Forms in Templates**  
Django forms can be displayed using:  
- `{{ form.as_p }}` → Wraps each field in `<p>` tags.  
- `{{ form.as_table }}` → Displays fields in a table.  
- `{{ form.as_ul }}` → Uses `<ul>` for formatting.  
- Manual rendering with `{{ form.field_name }}`.  

```html
<form method="post">  
    {% csrf_token %}  
    {{ form.as_p }}  
    <button type="submit">Submit</button>  
</form>  
```

### **Handling Form Submission in Views**  
Forms need to be processed in views by validating input and handling data.  

```python
from django.shortcuts import render  
from django.http import HttpResponse  
from .forms import ContactForm  

def contact_view(request):  
    if request.method == "POST":  
        form = ContactForm(request.POST)  
        if form.is_valid():  
            name = form.cleaned_data['name']  
            email = form.cleaned_data['email']  
            message = form.cleaned_data['message']  
            return HttpResponse("Form submitted successfully")  
    else:  
        form = ContactForm()  
    return render(request, "contact.html", {"form": form})  
```

### **Form Validation**  
Django automatically validates fields based on type constraints, but custom validation can be added using:  
- **Field-specific validation** via `clean_<field_name>()` methods.  
- **Global validation** using `clean()` method.  

```python
class ContactForm(forms.Form):  
    name = forms.CharField(max_length=100)  
    email = forms.EmailField()  

    def clean_name(self):  
        name = self.cleaned_data['name']  
        if "spam" in name.lower():  
            raise forms.ValidationError("Invalid name")  
        return name  
```

### **CSRF Protection**  
Django includes CSRF protection by default. Always use `{% csrf_token %}` inside `<form>` tags when handling form submissions.  

---

### **Conclusion**  
- `forms.Form` is used for manually defined forms.  
- It provides automatic validation and rendering tools.  
- Custom validation can be added using `clean_<field>()` or `clean()`.  
- Forms can be processed in views using `is_valid()` and `cleaned_data`.  
- CSRF protection should always be enabled for security.
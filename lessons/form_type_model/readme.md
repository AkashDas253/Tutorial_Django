## **Model Forms (`forms.ModelForm`) in Django**  

### **Overview**  
`forms.ModelForm` is a Django class that simplifies form creation by automatically generating fields based on a Django model. It eliminates redundancy by directly linking form fields to model fields.  

### **Key Features**  
- Automatically creates form fields based on a model.  
- Provides built-in validation using model constraints.  
- Allows customization of fields, widgets, and validation.  
- Supports saving form data directly to the database.  

### **Defining a Model Form**  
A `ModelForm` is created by inheriting from `forms.ModelForm` and specifying the associated model inside `Meta`:  

```python
from django import forms  
from .models import Contact  

class ContactForm(forms.ModelForm):  
    class Meta:  
        model = Contact  
        fields = ['name', 'email', 'message']  
```

### **Fields and Exclusions**  
By default, all model fields can be included, but specific fields can be controlled using:  

| Method | Description | Example |  
|--------|------------|---------|  
| `fields = '__all__'` | Includes all fields from the model. | `fields = '__all__'` |  
| `fields = ['field1', 'field2']` | Includes specific fields. | `fields = ['name', 'email']` |  
| `exclude = ['field1', 'field2']` | Excludes specific fields. | `exclude = ['created_at']` |  

### **Widgets in Model Forms**  
Custom widgets can be assigned to form fields using `widgets` in `Meta`:  

```python
class ContactForm(forms.ModelForm):  
    class Meta:  
        model = Contact  
        fields = ['name', 'email', 'message']  
        widgets = {  
            'message': forms.Textarea(attrs={'rows': 4, 'cols': 40})  
        }  
```

### **Customizing Labels and Help Texts**  
Django allows modifying field labels and adding help texts:  

```python
class ContactForm(forms.ModelForm):  
    class Meta:  
        model = Contact  
        fields = ['name', 'email']  
        labels = {  
            'name': 'Full Name'  
        }  
        help_texts = {  
            'email': 'Enter a valid email address.'  
        }  
```

### **Handling Model Forms in Views**  
A ModelForm can be processed in views like a standard form:  

```python
from django.shortcuts import render, redirect  
from .forms import ContactForm  

def contact_view(request):  
    if request.method == "POST":  
        form = ContactForm(request.POST)  
        if form.is_valid():  
            form.save()  # Saves the form data to the database  
            return redirect('success_page')  
    else:  
        form = ContactForm()  
    return render(request, "contact.html", {"form": form})  
```

### **Saving Model Forms with Custom Logic**  
Instead of using `form.save()`, custom logic can be added before saving:  

```python
def contact_view(request):  
    if request.method == "POST":  
        form = ContactForm(request.POST)  
        if form.is_valid():  
            contact = form.save(commit=False)  # Prevents immediate saving  
            contact.processed = True  # Custom logic before saving  
            contact.save()  
            return redirect('success_page')  
```

### **Form Validation**  
ModelForms automatically validate fields based on model constraints, but additional validation can be added using:  

#### **Field-Specific Validation**  
```python
class ContactForm(forms.ModelForm):  
    class Meta:  
        model = Contact  
        fields = ['email']  

    def clean_email(self):  
        email = self.cleaned_data.get('email')  
        if not email.endswith('@example.com'):  
            raise forms.ValidationError("Only @example.com emails are allowed.")  
        return email  
```

#### **Global Validation**  
```python
class ContactForm(forms.ModelForm):  
    class Meta:  
        model = Contact  
        fields = ['name', 'email']  

    def clean(self):  
        cleaned_data = super().clean()  
        name = cleaned_data.get('name')  
        email = cleaned_data.get('email')  

        if name and email and 'spam' in name.lower():  
            raise forms.ValidationError("Invalid name.")  
```

### **Rendering Model Forms in Templates**  
ModelForms can be rendered using Django’s form rendering methods:  

```html
<form method="post">  
    {% csrf_token %}  
    {{ form.as_p }}  
    <button type="submit">Submit</button>  
</form>  
```

### **Conclusion**  
- `forms.ModelForm` generates forms based on models.  
- It automatically creates form fields and enforces model constraints.  
- Customization can be done using `fields`, `exclude`, `widgets`, `labels`, and `help_texts`.  
- Forms can be saved directly using `form.save()`, with custom logic supported via `commit=False`.  
- Validation can be added using `clean_<field>()` or `clean()`.  
- ModelForms integrate seamlessly with Django’s templating and form-processing workflow.
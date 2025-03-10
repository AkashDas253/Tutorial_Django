## **Formsets in Django**  

### **Overview**  
A **formset** is a collection of multiple Django forms that share the same structure, allowing batch processing of forms in a single request. Formsets are useful for handling multiple instances of a model or dynamically adding/removing forms in a single page.  

### **Types of Formsets**  
Django provides two main types of formsets:  

| Formset Type | Description | Example Usage |  
|-------------|------------|--------------|  
| `formset_factory` | Used for managing multiple instances of a basic Django form (`forms.Form`). | Handling multiple email inputs in a single form. |  
| `modelformset_factory` | Used for managing multiple instances of a Django model (`forms.ModelForm`). | Managing multiple related database records in a single form submission. |  

---

## **1. Basic Formsets (`formset_factory`)**  
Used when managing multiple instances of a **regular Django form** (`forms.Form`).  

### **Defining a Formset**  
First, create a standard Django form:  

```python
from django import forms  

class ContactForm(forms.Form):  
    name = forms.CharField(max_length=100)  
    email = forms.EmailField()  
```

Next, create a formset using `formset_factory`:  

```python
from django.forms import formset_factory  

ContactFormSet = formset_factory(ContactForm, extra=2)  
```

### **Rendering Formset in a View**  
```python
from django.shortcuts import render  

def contact_view(request):  
    formset = ContactFormSet()  
    return render(request, "contact.html", {"formset": formset})  
```

### **Rendering Formset in a Template**  
```html
<form method="post">  
    {% csrf_token %}  
    {{ formset.management_form }}  
    {% for form in formset %}  
        {{ form.as_p }}  
    {% endfor %}  
    <button type="submit">Submit</button>  
</form>  
```

### **Processing a Formset in Views**  
```python
def contact_view(request):  
    if request.method == "POST":  
        formset = ContactFormSet(request.POST)  
        if formset.is_valid():  
            for form in formset:  
                print(form.cleaned_data)  # Process form data  
            return redirect('success_page')  
    else:  
        formset = ContactFormSet()  
    return render(request, "contact.html", {"formset": formset})  
```

---

## **2. Model Formsets (`modelformset_factory`)**  
Used when handling multiple instances of a **Django model** (`forms.ModelForm`).  

### **Defining a Model and ModelForm**  
```python
from django.db import models  

class Contact(models.Model):  
    name = models.CharField(max_length=100)  
    email = models.EmailField()  
```

```python
from django import forms  

class ContactForm(forms.ModelForm):  
    class Meta:  
        model = Contact  
        fields = ['name', 'email']  
```

### **Creating a ModelFormSet**  
```python
from django.forms import modelformset_factory  

ContactFormSet = modelformset_factory(Contact, form=ContactForm, extra=2)  
```

### **Rendering ModelFormSet in a View**  
```python
from django.shortcuts import render  

def contact_view(request):  
    formset = ContactFormSet(queryset=Contact.objects.all())  
    return render(request, "contact.html", {"formset": formset})  
```

### **Processing ModelFormSet in Views**  
```python
def contact_view(request):  
    if request.method == "POST":  
        formset = ContactFormSet(request.POST)  
        if formset.is_valid():  
            formset.save()  # Saves all form instances to the database  
            return redirect('success_page')  
    else:  
        formset = ContactFormSet(queryset=Contact.objects.all())  
    return render(request, "contact.html", {"formset": formset})  
```

---

## **3. Formset Management**  
### **Management Form**  
Django **requires a management form** to track the number of forms submitted.  
Ensure the following line is present in the template:  

```html
{{ formset.management_form }}
```

### **Customizing Formset Behavior**  
| Parameter | Description | Example |  
|-----------|------------|---------|  
| `extra` | Number of empty forms to display. | `formset_factory(ContactForm, extra=3)` |  
| `max_num` | Maximum number of forms allowed. | `formset_factory(ContactForm, max_num=5)` |  
| `can_delete` | Adds a checkbox for deleting forms. | `modelformset_factory(Contact, can_delete=True)` |  

---

## **4. Inline Formsets (Related Models)**  
Django provides `inlineformset_factory` for handling related models efficiently.  

### **Example: Parent-Child Relationship**  
Assume a `Book` model and a related `Author` model:  

```python
class Book(models.Model):  
    title = models.CharField(max_length=200)  

class Author(models.Model):  
    book = models.ForeignKey(Book, on_delete=models.CASCADE)  
    name = models.CharField(max_length=100)  
```

### **Creating an Inline Formset**  
```python
from django.forms import inlineformset_factory  

AuthorFormSet = inlineformset_factory(Book, Author, fields=['name'], extra=2)  
```

### **Using Inline Formset in Views**  
```python
def book_view(request, book_id):  
    book = Book.objects.get(id=book_id)  
    formset = AuthorFormSet(instance=book)  
    return render(request, "book_form.html", {"formset": formset})  
```

---

## **Conclusion**  
- **Formsets** allow handling multiple forms in a single request.  
- **Regular Formsets (`formset_factory`)** handle non-model forms.  
- **Model Formsets (`modelformset_factory`)** handle database models.  
- **Inline Formsets (`inlineformset_factory`)** manage related models.  
- **Management forms** ensure proper form tracking.  
- **Customization** using `extra`, `max_num`, and `can_delete` enhances flexibility.
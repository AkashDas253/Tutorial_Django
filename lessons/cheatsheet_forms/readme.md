## **Django Forms Cheatsheet**  

### **Basic Form Syntax**  
```python
from django import forms  

class MyForm(forms.Form):  
    name = forms.CharField(max_length=100)  
    email = forms.EmailField()  
    age = forms.IntegerField(required=False)  
```

---

### **ModelForm Syntax**  
```python
from django.forms import ModelForm  
from .models import UserProfile  

class UserProfileForm(ModelForm):  
    class Meta:  
        model = UserProfile  
        fields = ['name', 'email', 'age']  
```

---

### **Field Types**  
| Field Type | Description |
|------------|-------------|
| `CharField()` | Text input |
| `EmailField()` | Email input |
| `IntegerField()` | Numeric input |
| `BooleanField()` | Checkbox |
| `DateField()` | Date input |
| `ChoiceField()` | Dropdown selection |
| `FileField()` | File upload |

---

### **Rendering Forms in Templates**  
```html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}  <!-- Other options: form.as_table, form.as_ul -->
    <button type="submit">Submit</button>
</form>
```

---

### **Custom Form Validation**  
```python
class MyForm(forms.Form):  
    age = forms.IntegerField()  

    def clean_age(self):  
        age = self.cleaned_data.get('age')  
        if age < 18:  
            raise forms.ValidationError("Must be 18 or older.")  
        return age  
```

---

### **Widgets for Custom Input Styles**  
```python
class CustomForm(forms.Form):  
    name = forms.CharField(widget=forms.TextInput(attrs={'class': 'form-control'}))  
    password = forms.CharField(widget=forms.PasswordInput())  
```

---

### **Handling Form Submission in Views**  
```python
def my_view(request):  
    if request.method == "POST":  
        form = MyForm(request.POST)  
        if form.is_valid():  
            # Process form data
            return redirect('success')  
    else:  
        form = MyForm()  
    return render(request, 'form_template.html', {'form': form})  
```

---

### **Formsets (Multiple Forms Handling)**  
```python
from django.forms import formset_factory  

MyFormSet = formset_factory(MyForm, extra=2)  
formset = MyFormSet()  
```

---

### **Best Practices**  
| Best Practice | Benefit |
|--------------|---------|
| Use `ModelForm` when working with models | Reduces repetitive code |
| Always include `{% csrf_token %}` | Prevents CSRF attacks |
| Use built-in validators | Ensures data integrity |
| Customize widgets for better UX | Improves form styling |
| Validate form data in `clean_<field>()` | Ensures field-specific rules |

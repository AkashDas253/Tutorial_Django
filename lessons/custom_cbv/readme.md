## **Custom Class-Based Views (CBVs)**  

### **Definition**  
Custom CBVs extend Django's `View` class or generic CBVs to add specific behaviors and reusability across different views.

---

### **Creating a Custom CBV from Scratch**  

#### **Basic Custom CBV**  
```python
from django.http import HttpResponse
from django.views import View

class CustomView(View):
    def get(self, request, *args, **kwargs):
        return HttpResponse("Custom GET response")

    def post(self, request, *args, **kwargs):
        return HttpResponse("Custom POST response")
```

#### **URL Mapping**  
```python
from django.urls import path
from .views import CustomView

urlpatterns = [
    path('custom/', CustomView.as_view(), name='custom_view'),
]
```

---

### **Extending Generic CBVs for Custom Behavior**  

#### **Extending `ListView` with Custom Queryset Filtering**  
```python
from django.views.generic import ListView
from .models import MyModel

class CustomListView(ListView):
    model = MyModel
    template_name = 'my_template.html'

    def get_queryset(self):
        return MyModel.objects.filter(active=True)  # Custom filter
```

---

### **Custom CBV with Mixins**  

#### **Creating a Logging Mixin**  
```python
class LoggingMixin:
    def dispatch(self, request, *args, **kwargs):
        print(f"View accessed: {self.__class__.__name__}")
        return super().dispatch(request, *args, **kwargs)
```

#### **Using the Custom Mixin in a CBV**  
```python
class LoggedView(LoggingMixin, View):
    def get(self, request):
        return HttpResponse("View with logging.")
```

---

### **Custom CBV with Multiple Request Methods**  

#### **Handling Multiple HTTP Methods Dynamically**  
```python
class MultiMethodView(View):
    def get(self, request, *args, **kwargs):
        return HttpResponse("Handled GET request")

    def post(self, request, *args, **kwargs):
        return HttpResponse("Handled POST request")

    def put(self, request, *args, **kwargs):
        return HttpResponse("Handled PUT request")

    def delete(self, request, *args, **kwargs):
        return HttpResponse("Handled DELETE request")
```

---

### **Custom Form Handling in CBVs**  

#### **Extending `FormView` for Custom Validation**  
```python
from django.views.generic.edit import FormView
from django.http import HttpResponseRedirect
from .forms import MyForm

class CustomFormView(FormView):
    form_class = MyForm
    template_name = 'form_template.html'
    success_url = '/success/'

    def form_valid(self, form):
        # Custom validation logic
        form.save()
        return super().form_valid(form)

    def form_invalid(self, form):
        return super().form_invalid(form)
```

---

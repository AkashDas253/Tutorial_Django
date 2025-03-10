## **Custom Class-Based Views (CBVs) in Django**  

### **Overview**  
Django’s Class-Based Views (CBVs) provide a structured way to define views by encapsulating logic within reusable classes. While Django offers built-in CBVs like `ListView` and `DetailView`, custom CBVs allow greater flexibility by overriding methods or creating entirely new view classes.

---

### **Why Use Custom CBVs?**  
- Promotes reusability and modularity  
- Reduces redundant code in views  
- Provides a structured approach to handling HTTP requests  
- Allows easy method overriding for customized behavior  

---

### **Creating a Custom CBV from `View`**  
Django provides the base `View` class, which can be subclassed to create custom CBVs.  

```python
from django.http import HttpResponse
from django.views import View

class CustomView(View):
    def get(self, request, *args, **kwargs):
        return HttpResponse("Hello from a custom CBV!")
```
- Handles `GET` requests explicitly.  
- Can be extended with other HTTP methods like `post()`, `put()`, and `delete()`.  

---

### **Overriding HTTP Methods in CBVs**  
A custom CBV can handle multiple HTTP methods by defining respective methods (`get()`, `post()`, etc.).  

```python
from django.http import JsonResponse

class MultiMethodView(View):
    def get(self, request, *args, **kwargs):
        return JsonResponse({"message": "GET request received"})

    def post(self, request, *args, **kwargs):
        return JsonResponse({"message": "POST request received"})
```
- Processes both `GET` and `POST` requests differently.  
- `JsonResponse` is used to return structured JSON data.  

---

### **Customizing Template Views**  
Custom CBVs can extend `TemplateView` for rendering templates with extra context.  

```python
from django.views.generic import TemplateView

class CustomTemplateView(TemplateView):
    template_name = "custom.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["custom_message"] = "Welcome to the custom view!"
        return context
```
- Overrides `get_context_data()` to inject additional context variables.  

---

### **Creating Custom CBVs with Mixins**  
Mixins allow reusable logic across multiple views.  

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class SecureView(LoginRequiredMixin, TemplateView):
    template_name = "secure.html"
```
- Ensures only authenticated users can access the page.  

---

### **Custom Form Processing CBV**  
Custom CBVs can handle form submission by extending `FormView`.  

```python
from django.views.generic.edit import FormView
from .forms import ContactForm

class ContactFormView(FormView):
    template_name = "contact.html"
    form_class = ContactForm
    success_url = "/thanks/"

    def form_valid(self, form):
        # Custom processing logic
        return super().form_valid(form)
```
- Automatically handles form validation and redirection on success.  
- `form_valid()` can be overridden to add custom logic.  

---

### **Custom List View with Extra Filtering**  
Extending `ListView` allows custom query filtering.  

```python
from django.views.generic import ListView
from .models import Product

class FilteredProductListView(ListView):
    model = Product
    template_name = "product_list.html"

    def get_queryset(self):
        return Product.objects.filter(is_available=True)
```
- `get_queryset()` customizes how data is retrieved and filtered.  

---

### **Custom Detail View with Additional Context**  
A `DetailView` can be customized to return extra context for templates.  

```python
from django.views.generic import DetailView
from .models import Product

class ProductDetailView(DetailView):
    model = Product
    template_name = "product_detail.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["related_products"] = Product.objects.exclude(id=self.object.id)[:5]
        return context
```
- Injects related products into the template context.  

---

### **Best Practices for Custom CBVs**  
| Best Practice | Reason |
|--------------|--------|
| Use CBVs when reusability is needed | Reduces redundant code and improves maintainability. |
| Keep methods small and focused | Ensures better readability and debugging. |
| Use Mixins for reusable behaviors | Promotes modular design. |
| Override `get_context_data()` for extra data | Provides flexibility in passing context to templates. |
| Prefer Django’s generic CBVs over creating from scratch | Leverages built-in functionality to save development time. |

---
## **Generic Display Views in Django**  

### **Overview**  
Generic Display Views in Django provide prebuilt class-based views (CBVs) to handle the retrieval and display of data. These views simplify common tasks like listing objects, displaying details, and rendering static templates while promoting reusable and maintainable code.

---

### **Types of Generic Display Views**  
| View | Purpose |
|------|---------|
| `ListView` | Displays a list of objects from a model. |
| `DetailView` | Displays details of a single object. |
| `TemplateView` | Renders a static template with optional context. |

Each of these views inherits from `View` and `TemplateResponseMixin`, enabling them to handle HTTP requests and render templates.

---

### **ListView (Displaying Lists of Objects)**  
`ListView` retrieves multiple objects from a model and renders them in a template.

```python
from django.views.generic import ListView
from .models import Product

class ProductListView(ListView):
    model = Product
    template_name = "product_list.html"
    context_object_name = "products"
    paginate_by = 10
```

- `model`: Defines the model to fetch data from.  
- `template_name`: Specifies the template for rendering.  
- `context_object_name`: Sets the name for the object list in the template.  
- `paginate_by`: Enables pagination for large datasets.  

#### **Custom Queryset Filtering**  
`ListView` retrieves all objects by default. The `get_queryset()` method allows filtering.

```python
class AvailableProductListView(ListView):
    model = Product
    template_name = "available_products.html"

    def get_queryset(self):
        return Product.objects.filter(is_available=True)
```

- Fetches only available products.

#### **Accessing Pagination Information in Templates**  
Django’s pagination system provides details in templates.

```html
{% for product in products %}
    <p>{{ product.name }}</p>
{% endfor %}

{% if is_paginated %}
    {% if page_obj.has_previous %}
        <a href="?page={{ page_obj.previous_page_number }}">Previous</a>
    {% endif %}
    
    <span>Page {{ page_obj.number }} of {{ paginator.num_pages }}</span>
    
    {% if page_obj.has_next %}
        <a href="?page={{ page_obj.next_page_number }}">Next</a>
    {% endif %}
{% endif %}
```

---

### **DetailView (Displaying a Single Object)**  
`DetailView` retrieves and displays a single model instance.

```python
from django.views.generic import DetailView
from .models import Product

class ProductDetailView(DetailView):
    model = Product
    template_name = "product_detail.html"
    context_object_name = "product"
```

- `context_object_name`: Defines the variable name for the object in the template.

#### **Using Slug Instead of ID**  
For URL patterns using slugs instead of primary keys:

```python
class ProductDetailView(DetailView):
    model = Product
    template_name = "product_detail.html"
    slug_field = "slug"
    slug_url_kwarg = "slug"
```

#### **Adding Extra Context Data**  
Overriding `get_context_data()` to include additional data:

```python
class ProductDetailView(DetailView):
    model = Product
    template_name = "product_detail.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["related_products"] = Product.objects.exclude(id=self.object.id)[:5]
        return context
```

---

### **TemplateView (Rendering Static Pages)**  
`TemplateView` is useful for rendering static templates that do not require database queries.

```python
from django.views.generic import TemplateView

class AboutView(TemplateView):
    template_name = "about.html"
```

#### **Passing Custom Context Data**  
Adding extra variables to the context:

```python
class AboutView(TemplateView):
    template_name = "about.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["company_name"] = "TechCorp"
        return context
```

---

### **Best Practices for Generic Display Views**  
| Best Practice | Reason |
|--------------|--------|
| Use `ListView` for collections of objects | Reduces redundant code. |
| Use `DetailView` for individual objects | Simplifies object retrieval. |
| Use `TemplateView` for static pages | Avoids unnecessary database queries. |
| Override `get_queryset()` for custom filtering | Improves data retrieval control. |
| Enable pagination for large datasets | Enhances performance. |

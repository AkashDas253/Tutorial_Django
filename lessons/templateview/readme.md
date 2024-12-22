## **`TemplateView` in Django**

`TemplateView` is one of the most basic class-based views (CBVs) in Django. It renders a template without needing any form processing or model interaction. It's mainly used when you want to display static content or pass data to a template.

#### **Syntax for `TemplateView`**

```python
from django.views.generic import TemplateView

class MyTemplateView(TemplateView):
    template_name = "my_template.html"  # The template to render

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)  # Get the existing context
        context['my_variable'] = "This is a custom value"  # Add custom context
        return context
```

---

### **Key Attributes and Methods**

| **Attribute/Method**    | **Description**                                                                                             | **Example**                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| `template_name`         | The name of the template to render. This is the only required attribute.                                      | `template_name = "home.html"`             |
| `get_context_data()`    | Method used to add additional context variables to the template.                                             | `context['var'] = "Custom data"`          |
| `extra_context`         | Optional attribute for passing additional context variables (can be a dictionary). This is an alternative to `get_context_data()`. | `extra_context = {'key': 'value'}`        |

---

### **Example 1: Basic `TemplateView`**

This example shows a simple view rendering a template.

```python
from django.views.generic import TemplateView

class HomeView(TemplateView):
    template_name = "home.html"
```

- **Template**: `home.html` will be rendered without any additional context.
  
---

### **Example 2: Adding Custom Context Data**

You can add custom context data using `get_context_data()`. This allows you to pass dynamic content to the template.

```python
from django.views.generic import TemplateView

class AboutView(TemplateView):
    template_name = "about.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['company_name'] = "My Awesome Company"
        context['year'] = 2024
        return context
```

- **Template**: `about.html` will be rendered with the context:
  ```html
  <h1>{{ company_name }}</h1>
  <p>Founded in {{ year }}</p>
  ```

---

### **Example 3: Using `extra_context`**

Instead of overriding `get_context_data()`, you can directly use the `extra_context` attribute to pass additional data.

```python
from django.views.generic import TemplateView

class ContactView(TemplateView):
    template_name = "contact.html"
    extra_context = {
        'phone_number': '123-456-7890',
        'email': 'contact@mycompany.com'
    }
```

- **Template**: `contact.html` will be rendered with the context:
  ```html
  <p>Phone: {{ phone_number }}</p>
  <p>Email: {{ email }}</p>
  ```

---

### **Example 4: Using TemplateView with URLConf**

You typically associate `TemplateView` with a URL pattern to render it in response to an HTTP request.

```python
from django.urls import path
from .views import AboutView

urlpatterns = [
    path('about/', AboutView.as_view(), name='about'),
]
```

When a user visits `/about/`, Django will render the `AboutView` template.

---

### **Handling Context with Query Parameters**

You can also retrieve query parameters from the request and pass them to the context.

```python
from django.views.generic import TemplateView

class SearchView(TemplateView):
    template_name = "search_results.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        search_query = self.request.GET.get('query', '')
        context['search_query'] = search_query
        return context
```

- **Template**: `search_results.html`:
  ```html
  <h1>Search Results for: {{ search_query }}</h1>
  ```

If the user visits `/search/?query=django`, the template will display `Search Results for: django`.

---

### **Summary of Common Use Cases for `TemplateView`**

1. **Render a Static Template**: Simply render a template with no extra data.
   ```python
   class StaticView(TemplateView):
       template_name = "static_page.html"
   ```

2. **Pass Custom Context**: Add custom variables to the template context.
   ```python
   class CustomContextView(TemplateView):
       template_name = "custom.html"

       def get_context_data(self, **kwargs):
           context = super().get_context_data(**kwargs)
           context['some_var'] = 'Some dynamic content'
           return context
   ```

3. **Context from Query Parameters**: Dynamically pass values from the URL query string to the template.
   ```python
   class SearchResultsView(TemplateView):
       template_name = "search_results.html"

       def get_context_data(self, **kwargs):
           query = self.request.GET.get('query', 'default')
           context = super().get_context_data(**kwargs)
           context['query'] = query
           return context
   ```

---

`TemplateView` is a great way to quickly render templates with or without dynamic content. It simplifies the process of displaying content without requiring a lot of boilerplate code.

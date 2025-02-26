## Base View Class

### **Syntax for Base View**
The `View` class is the foundation for all Django class-based views. Here's the general syntax:

```python
from django.views import View
from django.http import HttpResponse

class MyBaseView(View):
    http_method_names = ['get', 'post', 'put', 'delete']  # Optional: Restrict allowed HTTP methods

    def get(self, request, *args, **kwargs):
        return HttpResponse("Handling GET request")

    def post(self, request, *args, **kwargs):
        return HttpResponse("Handling POST request")

    def put(self, request, *args, **kwargs):
        return HttpResponse("Handling PUT request")

    def delete(self, request, *args, **kwargs):
        return HttpResponse("Handling DELETE request")
```

---

### **Components of Base View**

#### 1. **Attributes**
| Attribute              | Description                                                                                 | Example                                  |
|-------------------------|---------------------------------------------------------------------------------------------|------------------------------------------|
| `http_method_names`     | Restricts the view to handle specific HTTP methods.                                         | `['get', 'post']`                        |
| `dispatch()`            | Main entry point for a request. Calls the appropriate handler (`get`, `post`, etc.).       | Used internally, often overridden.       |

---

#### 2. **HTTP Methods**
| Method      | Description                                      | Example                                           |
|-------------|--------------------------------------------------|--------------------------------------------------|
| `get`       | Handles GET requests.                           | `def get(self, request, *args, **kwargs):`       |
| `post`      | Handles POST requests.                          | `def post(self, request, *args, **kwargs):`      |
| `put`       | Handles PUT requests.                           | `def put(self, request, *args, **kwargs):`       |
| `delete`    | Handles DELETE requests.                        | `def delete(self, request, *args, **kwargs):`    |
| `head`      | Handles HEAD requests.                          | `def head(self, request, *args, **kwargs):`      |
| `options`   | Handles OPTIONS requests.                       | `def options(self, request, *args, **kwargs):`   |

---

#### 3. **Overridable Methods**
| Method        | Description                                                                                            | Example                                                                                  |
|---------------|--------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| `dispatch`    | Determines the HTTP method and calls the appropriate method (`get`, `post`, etc.).                     | Used for preprocessing requests or access control.                                       |
| `setup`       | Initializes attributes like `request`, `args`, and `kwargs`.                                            | Override to customize attribute setup.                                                   |
| `http_method_not_allowed` | Called if an unsupported HTTP method is used.                                              | Customizes the 405 error response.                                                       |

---

#### 4. **Customizations**
| Customization               | Description                                                | Example                                                 |
|-----------------------------|------------------------------------------------------------|---------------------------------------------------------|
| Restrict Methods            | Limit allowed HTTP methods using `http_method_names`.       | `http_method_names = ['get', 'post']`                  |
| Add Middleware              | Use decorators with `dispatch()` to apply middleware-like functionality. | `@method_decorator(csrf_exempt, name='dispatch')`       |
| Logging or Access Control   | Customize `dispatch()` to add logging or authentication.    | Add a `print()` or check permissions in `dispatch()`.   |

---

### **Examples of Different Cases**

#### Case 1: Restrict to GET and POST
```python
class RestrictedView(View):
    http_method_names = ['get', 'post']

    def get(self, request, *args, **kwargs):
        return HttpResponse("GET method allowed")

    def post(self, request, *args, **kwargs):
        return HttpResponse("POST method allowed")
```

---

#### Case 2: Preprocessing in `dispatch`
```python
class PreprocessView(View):
    def dispatch(self, request, *args, **kwargs):
        print("Preprocessing request")
        return super().dispatch(request, *args, **kwargs)

    def get(self, request, *args, **kwargs):
        return HttpResponse("GET with preprocessing")
```

---

#### Case 3: Handling Unsupported Methods
```python
class Custom405View(View):
    def http_method_not_allowed(self, request, *args, **kwargs):
        return HttpResponse("Custom 405 Error: Method not allowed", status=405)

    def get(self, request, *args, **kwargs):
        return HttpResponse("GET method")
```

---

### **Table: Summary of Usage**

| Feature               | Attribute/Method           | Example Code                                            |
|-----------------------|----------------------------|--------------------------------------------------------|
| Restrict Methods      | `http_method_names`        | `http_method_names = ['get', 'post']`                 |
| Preprocessing         | `dispatch()`              | Add logging, checks, or middleware.                   |
| Handle GET            | `get(self, request, ...)`  | `def get(self, request, *args, **kwargs):`            |
| Handle POST           | `post(self, request, ...)` | `def post(self, request, *args, **kwargs):`           |
| Unsupported Methods   | `http_method_not_allowed`  | `return HttpResponse("Not allowed", status=405)`      |
| Custom Setup          | `setup(self, ...)`         | Add attributes like `self.user = request.user`.       |

---

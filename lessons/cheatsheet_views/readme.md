
# Django Views  

### Overview  
Django views handle HTTP requests and return HTTP responses. They define the logic for processing requests and generating responses.  

---

## **Function-Based Views (FBVs)**  

### **Definition**  
Function-Based Views (FBVs) are simple Python functions that handle HTTP requests and return HTTP responses. They provide explicit control over request handling and allow decorators for additional functionality.  

---

### **Syntax**  
```python
from django.http import HttpResponse
from django.views.decorators.http import require_http_methods
from django.contrib.auth.decorators import login_required, permission_required
from django.views.decorators.csrf import csrf_exempt

@require_http_methods(["GET", "POST"])
@login_required
@permission_required('app.permission')
@csrf_exempt
def view_name(request, *args, **kwargs):
    # Logic
    return HttpResponse("Response content")
```

---

### **URL Mapping**  
```python
from django.urls import path
from .views import view_name

urlpatterns = [
    path('route/', view_name, name='view_name'),
]
```

---

### **Handling Request Methods**  
FBVs explicitly check for request methods using `if` statements.  

```python
from django.http import JsonResponse

def my_view(request):
    if request.method == "GET":
        return JsonResponse({"message": "GET request received"})
    elif request.method == "POST":
        return JsonResponse({"message": "POST request received"})
    return JsonResponse({"error": "Method not allowed"}, status=405)
```

---

### **Rendering Templates**  
Django's `render()` function simplifies template rendering in FBVs.  

```python
from django.shortcuts import render

def my_template_view(request):
    context = {"name": "Alice"}
    return render(request, "template.html", context)
```

---

### **Redirecting Users**  
Redirect users to another view or URL using `redirect()`.  

```python
from django.shortcuts import redirect

def my_redirect_view(request):
    return redirect("home_page")
```

---

### **Handling 404 Errors**  
Use `get_object_or_404()` to automatically return a 404 response if an object is not found.  

```python
from django.shortcuts import get_object_or_404
from .models import MyModel

def my_detail_view(request, pk):
    obj = get_object_or_404(MyModel, pk=pk)
    return render(request, "detail.html", {"object": obj})
```

---

### **Decorators in FBVs**  

| Decorator | Purpose |
|-----------|---------|
| `@login_required` | Restricts access to authenticated users. |
| `@permission_required('app.permission')` | Ensures the user has specific permissions. |
| `@require_http_methods(["GET", "POST"])` | Restricts the view to specified HTTP methods. |
| `@csrf_exempt` | Disables CSRF protection for the view. |
| `@staff_member_required` | Restricts access to staff users. |
| `@user_passes_test(lambda u: u.is_superuser)` | Allows access only to users passing a custom test. |

---

---

## **Class-Based Views (CBVs)**  

### **Definition**  
Class-Based Views (CBVs) use Python classes instead of functions to structure views. They promote reusability and maintainability by leveraging object-oriented programming principles. CBVs can inherit from Django's built-in generic views or be customized for specific logic.  

---

### **Syntax**  
```python
from django.views import View
from django.http import HttpResponse
from django.contrib.auth.mixins import LoginRequiredMixin, PermissionRequiredMixin, UserPassesTestMixin
from django.utils.decorators import method_decorator
from django.views.decorators.csrf import csrf_exempt

@method_decorator(csrf_exempt, name='dispatch')
class ViewName(LoginRequiredMixin, PermissionRequiredMixin, UserPassesTestMixin, View):
    permission_required = 'app.permission'

    def get(self, request, *args, **kwargs):
        # GET request logic
        return HttpResponse("Response content")

    def post(self, request, *args, **kwargs):
        # POST request logic
        return HttpResponse("Response content")
```

---

### **URL Mapping**  
```python
from django.urls import path
from .views import ViewName

urlpatterns = [
    path('route/', ViewName.as_view(), name='view_name'),
]
```

---

### **Built-in Generic Views**  

| Category | View Type | Purpose |
|----------|----------|---------|
| **Basic Views** | `View` | Base class for all CBVs. |
| **Generic Display Views** | `TemplateView` | Renders a template. |
| | `ListView` | Displays a list of objects. |
| | `DetailView` | Shows details of a single object. |
| **Generic Editing Views** | `CreateView` | Handles object creation. |
| | `UpdateView` | Edits an existing object. |
| | `DeleteView` | Removes an object. |
| **Generic Form Views** | `FormView` | Handles form submission. |
| **Generic Redirect Views** | `RedirectView` | Redirects users to another URL. |

---

### **CBV Syntax Components**  

| Component | Purpose |
|-----------|---------|
| `model` | Specifies the model used in the view. |
| `template_name` | Defines the template file to be rendered. |
| `context_object_name` | Names the object in the template context. |
| `queryset` | Defines a custom query set. |
| `form_class` | Specifies the form class in form-related views. |
| `success_url` | Defines the URL to redirect to after form submission. |
| `get_context_data(self, **kwargs)` | Adds extra context variables. |
| `get_queryset(self)` | Customizes the queryset. |
| `get_object(self, queryset=None)` | Retrieves a single object instance. |
| `form_valid(self, form)` | Custom logic on successful form submission. |
| `form_invalid(self, form)` | Custom logic when form submission fails. |

---

### **Handling Request Methods in CBVs**  
CBVs define methods like `get()`, `post()`, `put()`, and `delete()` for handling different request types.  

```python
from django.views import View
from django.http import JsonResponse

class MyView(View):
    def get(self, request, *args, **kwargs):
        return JsonResponse({"message": "GET request received"})

    def post(self, request, *args, **kwargs):
        return JsonResponse({"message": "POST request received"})
```

---

### **Rendering Templates in CBVs**  
```python
from django.views.generic import TemplateView

class MyTemplateView(TemplateView):
    template_name = "template.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["name"] = "Alice"
        return context
```

---

### **Redirecting Users in CBVs**  
```python
from django.views.generic import RedirectView

class MyRedirectView(RedirectView):
    pattern_name = 'home_page'
```

---

### **Handling 404 Errors in CBVs**  
```python
from django.views.generic import DetailView
from django.shortcuts import get_object_or_404
from .models import MyModel

class MyDetailView(DetailView):
    model = MyModel
    template_name = "detail.html"

    def get_object(self, queryset=None):
        return get_object_or_404(MyModel, pk=self.kwargs["pk"])
```

---

### **Decorators in CBVs**  
CBVs use `method_decorator()` to apply function-based decorators to specific methods.  

| Decorator | Purpose |
|-----------|---------|
| `@method_decorator(login_required, name='dispatch')` | Restricts access to authenticated users. |
| `@method_decorator(permission_required('app.permission'), name='dispatch')` | Ensures the user has specific permissions. |
| `@method_decorator(csrf_exempt, name='dispatch')` | Disables CSRF protection. |
| `@method_decorator(staff_member_required, name='dispatch')` | Restricts access to staff users. |

---

### **Mixins in CBVs**  

| Mixin | Purpose |
|--------|---------|
| `LoginRequiredMixin` | Ensures the user is logged in. |
| `PermissionRequiredMixin` | Restricts access based on permissions. |
| `UserPassesTestMixin` | Allows access only if a condition is met. |

Example:  
```python
from django.contrib.auth.mixins import LoginRequiredMixin

class MySecureView(LoginRequiredMixin, View):
    def get(self, request, *args, **kwargs):
        return HttpResponse("Secure content")
```

---

### **Custom CBVs**  
Custom CBVs extend Django’s `View` class and define reusable logic.  

```python
class CustomBaseView(View):
    def dispatch(self, request, *args, **kwargs):
        # Custom logic before request handling
        response = super().dispatch(request, *args, **kwargs)
        # Custom logic after request handling
        return response
```

| Custom CBV | Purpose |
|------------|---------|
| `CustomBaseView` | A reusable base class for adding custom logic. |
| `LoginRequiredCustomView` | A CBV that enforces authentication. |
| `APIBaseView` | A base CBV for API responses. |

---

---

## **Handling Request Methods in Django Views**  

### **Definition**  
Django views handle different HTTP request methods to process user interactions. Function-Based Views (FBVs) use conditionals to check request types, while Class-Based Views (CBVs) define dedicated methods for each request type.

---

### **Handling Request Methods in FBVs**  
FBVs check request methods explicitly using `request.method`.  

```python
from django.http import JsonResponse

def my_view(request):
    if request.method == "GET":
        return JsonResponse({"message": "GET request received"})
    elif request.method == "POST":
        return JsonResponse({"message": "POST request received"})
    elif request.method == "PUT":
        return JsonResponse({"message": "PUT request received"})
    elif request.method == "DELETE":
        return JsonResponse({"message": "DELETE request received"})
    else:
        return JsonResponse({"error": "Method not allowed"}, status=405)
```

---

### **Handling Request Methods in CBVs**  
CBVs define separate methods for each HTTP request type.  

```python
from django.views import View
from django.http import JsonResponse

class MyView(View):
    def get(self, request, *args, **kwargs):
        return JsonResponse({"message": "GET request received"})

    def post(self, request, *args, **kwargs):
        return JsonResponse({"message": "POST request received"})

    def put(self, request, *args, **kwargs):
        return JsonResponse({"message": "PUT request received"})

    def delete(self, request, *args, **kwargs):
        return JsonResponse({"message": "DELETE request received"})
```

---

### **Handling Request Methods with Django Decorators**  
Django provides decorators to restrict views to specific HTTP methods.

#### **FBV with @require_http_methods()**
```python
from django.http import JsonResponse
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    return JsonResponse({"message": f"{request.method} request received"})
```

#### **CBV with @method_decorator()**
```python
from django.utils.decorators import method_decorator
from django.views.decorators.http import require_http_methods
from django.views import View

@method_decorator(require_http_methods(["GET", "POST"]), name="dispatch")
class MyView(View):
    def get(self, request, *args, **kwargs):
        return JsonResponse({"message": "GET request received"})

    def post(self, request, *args, **kwargs):
        return JsonResponse({"message": "POST request received"})
```

---

### **Handling Request Methods in Django REST Framework (DRF)**  
Django REST Framework (DRF) provides `APIView` to define API request handlers.

```python
from rest_framework.views import APIView
from rest_framework.response import Response

class MyAPIView(APIView):
    def get(self, request, *args, **kwargs):
        return Response({"message": "GET request received"})

    def post(self, request, *args, **kwargs):
        return Response({"message": "POST request received"})
```

---

### **HTTP Request Methods and Their Uses**  

| Method  | Description |
|---------|------------|
| `GET`   | Retrieve data from the server. |
| `POST`  | Submit data to the server. |
| `PUT`   | Update an existing resource. |
| `PATCH` | Partially update an existing resource. |
| `DELETE` | Remove a resource. |

---

---

## **Middleware Integration in Django Views**  

### **Definition**  
Middleware in Django is a framework-level hook that processes requests and responses globally before they reach views or after views return a response. Middleware functions operate at different stages of the request-response cycle.

---

### **Middleware Flow in Django**  
1. Request middleware modifies or processes the request before it reaches the view.  
2. View processes the request and generates a response.  
3. Response middleware modifies or processes the response before sending it to the client.  

---

### **Creating Custom Middleware**  
Custom middleware must define at least one of the following methods:  
- `__init__(self, get_response)`: Initializes the middleware.  
- `__call__(self, request)`: Processes the request before it reaches the view.  
- `process_view(request, view_func, view_args, view_kwargs)`: Modifies the request before it reaches the view.  
- `process_exception(request, exception)`: Handles exceptions raised in the view.  
- `process_template_response(request, response)`: Modifies template responses.  

```python
class CustomMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        # Before view execution
        print("Middleware: Before View")

        response = self.get_response(request)

        # After view execution
        print("Middleware: After View")

        return response
```

---

### **Registering Middleware in Django**  
Middleware must be added to the `MIDDLEWARE` list in `settings.py`.  

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'myapp.middleware.CustomMiddleware',  # Custom Middleware
]
```

---

### **Built-in Middleware in Django**  

| Middleware | Purpose |
|------------|---------|
| `SecurityMiddleware` | Enhances security by adding HTTP headers. |
| `SessionMiddleware` | Manages session data for users. |
| `CommonMiddleware` | Provides common utilities like URL redirection. |
| `CsrfViewMiddleware` | Enables CSRF protection. |
| `AuthenticationMiddleware` | Attaches user authentication data to requests. |
| `MessageMiddleware` | Enables temporary storage of user messages. |

---

### **Using Middleware in Django Views**  

#### **Applying Middleware to Specific Views**  
Django provides `decorators` to apply middleware at the view level instead of globally.

```python
from django.utils.decorators import decorator_from_middleware
from django.http import JsonResponse

class CustomMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        print("Middleware: Processing request")
        return self.get_response(request)

CustomMiddlewareDecorator = decorator_from_middleware(CustomMiddleware)

@CustomMiddlewareDecorator
def my_view(request):
    return JsonResponse({"message": "Hello, World!"})
```

---

### **Handling Middleware Exceptions**  

Middleware can handle errors globally before they reach views.

```python
class ExceptionHandlingMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        try:
            response = self.get_response(request)
        except Exception as e:
            return JsonResponse({"error": str(e)}, status=500)
        return response
```

---

## **Decorators in Django Views**  

### **Definition**  
Decorators in Django views are functions that modify the behavior of views without changing their code. They are often used for authentication, request validation, and method restrictions.

---

### **Common Decorators in Django Views**  

| Decorator | Purpose |
|-----------|---------|
| `@login_required` | Restricts access to authenticated users. |
| `@permission_required('app.permission')` | Ensures users have a specific permission. |
| `@user_passes_test(test_function)` | Restricts access based on a custom test function. |
| `@csrf_exempt` | Disables CSRF protection for a specific view. |
| `@require_http_methods(["GET", "POST"])` | Restricts allowed HTTP methods for the view. |
| `@require_GET` | Ensures only `GET` requests are allowed. |
| `@require_POST` | Ensures only `POST` requests are allowed. |
| `@require_safe` | Allows only `GET` and `HEAD` requests. |

---

### **Using Decorators in Function-Based Views (FBVs)**  

```python
from django.http import HttpResponse
from django.contrib.auth.decorators import login_required, permission_required, user_passes_test
from django.views.decorators.http import require_http_methods, require_GET, require_POST
from django.views.decorators.csrf import csrf_exempt

@require_http_methods(["GET", "POST"])
@login_required
@permission_required('app.view_model')
@user_passes_test(lambda user: user.is_superuser)
@csrf_exempt
def my_view(request):
    return HttpResponse("Hello, World!")
```

---

### **Using Decorators in Class-Based Views (CBVs)**  

Since CBVs use classes instead of functions, decorators must be applied using `method_decorator`.  

```python
from django.views import View
from django.utils.decorators import method_decorator
from django.contrib.auth.decorators import login_required, permission_required
from django.views.decorators.csrf import csrf_exempt

class MyView(View):
    @method_decorator(login_required)
    @method_decorator(permission_required('app.view_model'))
    @method_decorator(csrf_exempt)
    def dispatch(self, request, *args, **kwargs):
        return HttpResponse("Hello from CBV!")
```

---

### **Applying Decorators to All Methods in CBVs**  

Instead of applying decorators to each method, you can use `dispatch()` to apply them to all request methods.

```python
@method_decorator(login_required, name='dispatch')
@method_decorator(permission_required('app.view_model'), name='dispatch')
class MyView(View):
    def get(self, request):
        return HttpResponse("GET request received.")

    def post(self, request):
        return HttpResponse("POST request received.")
```

---

---
## **Mixins in Class-Based Views (CBVs)**  

### **Definition**  
Mixins in Django CBVs are reusable classes that add specific functionality to views. They allow multiple behaviors to be combined without modifying the base view class.

---

### **Common Django Mixins**  

| Mixin | Purpose |
|-------|---------|
| `LoginRequiredMixin` | Restricts access to authenticated users. |
| `PermissionRequiredMixin` | Ensures users have specific permissions. |
| `UserPassesTestMixin` | Restricts access based on a custom test function. |
| `CsrfExemptMixin` | Disables CSRF protection. |
| `FormMixin` | Adds form handling behavior to views. |
| `SingleObjectMixin` | Fetches a single object for views like `DetailView`. |
| `MultipleObjectMixin` | Fetches multiple objects for views like `ListView`. |

---

### **Using Mixins in CBVs**  

#### **Authentication and Permission Mixins**  

```python
from django.views import View
from django.http import HttpResponse
from django.contrib.auth.mixins import LoginRequiredMixin, PermissionRequiredMixin, UserPassesTestMixin

class MySecureView(LoginRequiredMixin, PermissionRequiredMixin, UserPassesTestMixin, View):
    permission_required = 'app.view_model'

    def test_func(self):
        return self.request.user.is_superuser  # Custom access condition

    def get(self, request):
        return HttpResponse("Only accessible to authorized users.")
```

---

#### **Form Handling with `FormMixin`**  

```python
from django.views.generic.edit import FormMixin
from django.views.generic import DetailView
from django.http import HttpResponseRedirect
from .models import MyModel
from .forms import MyForm

class MyDetailView(FormMixin, DetailView):
    model = MyModel
    form_class = MyForm
    template_name = 'my_template.html'
    
    def post(self, request, *args, **kwargs):
        form = self.get_form()
        if form.is_valid():
            return self.form_valid(form)
        return self.form_invalid(form)

    def form_valid(self, form):
        form.save()
        return HttpResponseRedirect(self.get_success_url())
```

---

### **Custom Mixins**  

#### **Creating a Custom Mixin**  

```python
class CustomLoggingMixin:
    def dispatch(self, request, *args, **kwargs):
        print(f"Accessing {self.__class__.__name__}")
        return super().dispatch(request, *args, **kwargs)
```

#### **Using a Custom Mixin in a View**  

```python
class MyLoggedView(CustomLoggingMixin, View):
    def get(self, request):
        return HttpResponse("View with custom logging.")
```

---

---

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

---

### **9. Django REST Framework (DRF) API Views**  
#### **9.1 Definition**  
- Role of API views in Django REST Framework  

#### **9.2 Function-Based API Views**  
- Using `@api_view()` and permissions  

#### **9.3 Class-Based API Views (APIView)**  
- Example implementation of `APIView`  

#### **9.4 ViewSets and Routers**  
##### **9.4.1 Difference Between APIView and ViewSet**  
- When to use `APIView` vs. `ViewSet`  

##### **9.4.2 Defining a ViewSet**  
- `ModelViewSet` example  

##### **9.4.3 URL Mapping with Routers**  
- Using `DefaultRouter` for API views  

---

## **Asynchronous Views in Django**  

### **Definition**  
Asynchronous views in Django allow handling requests asynchronously using Python's `async` and `await`, improving performance for I/O-bound tasks like API calls, database queries, and external service requests.

---

### **Key Features**  
- Improve performance for I/O-bound operations.  
- Reduce request blocking for high-traffic applications.  
- Compatible with Django’s ORM and third-party libraries supporting async.  

---

### **Basic Asynchronous View**  
```python
from django.http import JsonResponse
import asyncio

async def async_view(request):
    await asyncio.sleep(2)  # Simulating async operation
    return JsonResponse({"message": "Async response after delay"})
```

---

### **URL Mapping**  
```python
from django.urls import path
from .views import async_view

urlpatterns = [
    path('async/', async_view, name='async_view'),
]
```

---

### **Using Async Database Queries**  
Django 4.1+ supports async ORM queries using `await`.  
```python
from django.http import JsonResponse
from .models import MyModel

async def async_db_view(request):
    obj = await MyModel.objects.afirst()  # Async query
    return JsonResponse({"name": obj.name if obj else "No data"})
```

---

### **Using Async with External APIs**  
```python
import aiohttp
from django.http import JsonResponse

async def async_api_view(request):
    async with aiohttp.ClientSession() as session:
        async with session.get('https://api.example.com/data') as response:
            data = await response.json()
    return JsonResponse(data)
```

---

### **Mixing Async and Sync Code**  
Django runs views in a synchronous thread by default.  
Use `sync_to_async` to run sync functions inside async views.  
```python
from asgiref.sync import sync_to_async
from django.http import JsonResponse
from .models import MyModel

async def async_mixed_view(request):
    obj = await sync_to_async(MyModel.objects.get)(id=1)  # Convert sync to async
    return JsonResponse({"name": obj.name})
```

---

### **Limitations of Async Views**  
- Middleware, signals, and database transactions are still mostly synchronous.  
- Some third-party packages may not support async yet.  
- Running sync code inside async views can cause performance issues.  

---

---

## Django Views  

### Overview  
Django views handle HTTP requests and return HTTP responses. They define the logic for processing requests and generating responses.  

---

### Function-Based Views (FBVs)  

#### Definition:  
Function-based views (FBVs) are simple Python functions that take a request object as input and return a response. They provide explicit control over request handling.  

#### Syntax:  
```python
from django.http import HttpResponse
from django.views.decorators.http import require_http_methods
from django.contrib.auth.decorators import login_required, permission_required
from django.views.decorators.csrf import csrf_exempt

@require_http_methods(["GET", "POST"])  # Restrict to GET and POST requests
@login_required  # Ensure the user is authenticated
@permission_required('app.permission')  # Require specific permission
@csrf_exempt  # Disable CSRF protection
def view_name(request, *args, **kwargs):
    # Logic
    return HttpResponse("Response content")
```  

#### URL Mapping:  
```python
from django.urls import path
from .views import view_name

urlpatterns = [
    path('route/', view_name, name='view_name'),
]
```  

---

### Class-Based Views (CBVs)  

#### Definition:  
Class-based views (CBVs) use Python classes instead of functions to structure views. They allow better code reusability by using object-oriented principles.  

#### Syntax:  
```python
from django.views import View
from django.http import HttpResponse
from django.contrib.auth.mixins import LoginRequiredMixin, PermissionRequiredMixin, UserPassesTestMixin

class ViewName(LoginRequiredMixin, PermissionRequiredMixin, UserPassesTestMixin, View):
    permission_required = 'app.permission'  # Require specific permission

    def get(self, request, *args, **kwargs):
        # GET request logic
        return HttpResponse("Response content")

    def post(self, request, *args, **kwargs):
        # POST request logic
        return HttpResponse("Response content")
```  

#### URL Mapping:  
```python
from django.urls import path
from .views import ViewName

urlpatterns = [
    path('route/', ViewName.as_view(), name='view_name'),
]
```  

---

### Custom Class-Based Views  

#### Definition:  
Custom CBVs allow defining reusable view logic by extending Django's `View` class or generic views.  

#### Syntax:  
```python
from django.http import JsonResponse
from django.views import View

class CustomView(View):
    def dispatch(self, request, *args, **kwargs):
        # Custom logic before request handling
        response = super().dispatch(request, *args, **kwargs)
        # Custom logic after request handling
        return response

    def get(self, request, *args, **kwargs):
        return JsonResponse({"message": "GET request handled"})

    def post(self, request, *args, **kwargs):
        return JsonResponse({"message": "POST request handled"})
```

#### URL Mapping:  
```python
from django.urls import path
from .views import CustomView

urlpatterns = [
    path('custom/', CustomView.as_view(), name='custom_view'),
]
```

---

### Built-in Generic Views  

#### Definition:  
Django provides built-in generic views to handle common tasks like displaying objects, processing forms, and handling redirects, reducing boilerplate code.  

| Category | View Type | Purpose |
|----------|----------|---------|
| **Basic Views** | `View` | Base class for all CBVs. |
| **Generic Display Views** | `TemplateView` | Renders a template. |
| | `ListView` | Displays a list of objects. |
| | `DetailView` | Shows details of a single object. |
| **Generic Editing Views** | `CreateView` | Handles object creation. |
| | `UpdateView` | Edits an existing object. |
| | `DeleteView` | Removes an object. |
| **Generic Form Views** | `FormView` | Handles form submission. |
| **Generic Redirect Views** | `RedirectView` | Redirects users to another URL. |  

---

### CBV Syntax Components  

#### Definition:  
CBVs use attributes and methods to define behavior, allowing customization via mixins and inheritance.  

| Part | Description |
|------|------------|
| `model` | Specifies the model used in the view. |
| `template_name` | Defines the template file to be rendered. |
| `context_object_name` | Names the object in the template context. |
| `queryset` | Defines a custom query set. |
| `form_class` | Specifies the form class in form-related views. |
| `success_url` | Defines the URL to redirect to after form submission. |
| `get_context_data(self, **kwargs)` | Adds extra context variables. |
| `get_queryset(self)` | Customizes the queryset. |
| `get_object(self, queryset=None)` | Retrieves a single object instance. |
| `form_valid(self, form)` | Custom logic on successful form submission. |
| `form_invalid(self, form)` | Custom logic when form submission fails. |  

---

### Decorators in Django Views  

#### Definition:  
Decorators modify the behavior of views by adding authentication, permissions, or request method restrictions.  

| Decorator | Usage Location | Description |
|-----------|---------------|------------|
| `@login_required` | FBVs | Ensures the user is authenticated. |
| `@permission_required('app.permission')` | FBVs | Ensures the user has the required permission. |
| `@require_http_methods(["GET", "POST"])` | FBVs | Restricts allowed HTTP methods. |
| `@csrf_exempt` | FBVs | Disables CSRF protection for a view. |
| `@staff_member_required` | FBVs | Ensures the user is a staff member. |
| `@user_passes_test(lambda u: u.is_superuser)` | FBVs | Ensures the user passes a custom test. |
| `@api_view(['GET', 'POST'])` | DRF Views | Specifies allowed request methods in DRF views. |
| `@permission_classes([IsAuthenticated])` | DRF Views | Restricts access to authenticated users in DRF views. |

---

### Handling Request Methods  

#### Definition:  
HTTP request methods define the type of operation being performed on the server.  

| Method  | Description |
|---------|------------|
| `GET`   | Retrieve data. |
| `POST`  | Submit data. |
| `PUT`   | Update a resource. |
| `DELETE` | Remove a resource. |  

---

### Rendering Templates  

#### Definition:  
Templates generate dynamic HTML pages using Django's template engine.  

#### Syntax:  
```python
from django.shortcuts import render

def my_view(request):
    context = {"name": "Alice"}
    return render(request, "my_template.html", context)
```  

---

### Redirecting Users  

#### Definition:  
Redirects send users to a different URL, often after form submissions.  

#### Syntax:  
```python
from django.shortcuts import redirect

def my_redirect_view(request):
    return redirect("home_page")
```  

---

### Handling 404 Errors  

#### Definition:  
When an object is not found, Django raises a `404` error. `get_object_or_404()` simplifies this process.  

#### Syntax:  
```python
from django.shortcuts import get_object_or_404
from .models import MyModel

def my_detail_view(request, pk):
    obj = get_object_or_404(MyModel, pk=pk)
    return render(request, "detail.html", {"object": obj})
```  

---

### API Views with Django REST Framework (DRF)  

#### Definition:  
Django REST Framework (DRF) provides tools for building RESTful APIs with class-based API views.  

#### Syntax:  
```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.decorators import api_view, permission_classes
from rest_framework.permissions import IsAuthenticated

@api_view(['GET', 'POST'])
@permission_classes([IsAuthenticated])
def my_api_view(request):
    if request.method == 'GET':
        return Response({"message": "GET request received"})
    elif request.method == 'POST':
        return Response({"message": "POST request received"})
```  

#### URL Mapping:  
```python
from django.urls import path
from .views import my_api_view

urlpatterns = [
    path('api/', my_api_view, name='api_view'),
]
```  

---

### **Middleware Integration**  

Middleware processes requests and responses globally before reaching views.  

#### **Custom Middleware Example**  
```python
from django.utils.deprecation import MiddlewareMixin

class CustomMiddleware(MiddlewareMixin):
    def process_request(self, request):
        print("Request intercepted")
    
    def process_response(self, request, response):
        print("Response intercepted")
        return response
```

#### **Registering Middleware**  
Add it to `MIDDLEWARE` in `settings.py`:  
```python
MIDDLEWARE = [
    'myapp.middleware.CustomMiddleware',
    # Other middleware...
]
```

---

### **Custom Decorators for Views**  

#### **Definition**  
Custom decorators wrap views with reusable logic, such as access control or logging.

#### **Syntax**  
```python
from functools import wraps
from django.http import HttpResponseForbidden

def custom_decorator(view_func):
    @wraps(view_func)
    def wrapper(request, *args, **kwargs):
        if not request.user.is_authenticated:
            return HttpResponseForbidden("Access Denied")
        return view_func(request, *args, **kwargs)
    return wrapper
```

#### **Usage in FBV**  
```python
@custom_decorator
def my_view(request):
    return HttpResponse("View accessible")
```

#### **Usage in CBV**  
```python
from django.utils.decorators import method_decorator

class MyView(View):
    @method_decorator(custom_decorator)
    def get(self, request):
        return HttpResponse("CBV with custom decorator")
```

---

### **Mixins for CBVs**  

Mixins allow reusable functionality in CBVs.  

| Mixin | Purpose |
|--------|---------|
| `LoginRequiredMixin` | Restricts access to logged-in users. |
| `PermissionRequiredMixin` | Grants access based on permissions. |
| `UserPassesTestMixin` | Runs a custom test function before allowing access. |

#### **Example: Using Mixins in a CBV**  
```python
from django.contrib.auth.mixins import LoginRequiredMixin, PermissionRequiredMixin
from django.views import View

class SecureView(LoginRequiredMixin, PermissionRequiredMixin, View):
    permission_required = 'app.view_permission'

    def get(self, request):
        return HttpResponse("Secure Content")
```

---

### **Custom CBVs**  

Django allows defining custom CBVs to encapsulate logic.

#### **Custom Base View**
```python
from django.http import JsonResponse
from django.views import View

class CustomBaseView(View):
    def dispatch(self, request, *args, **kwargs):
        if not request.user.is_authenticated:
            return JsonResponse({"error": "Unauthorized"}, status=401)
        return super().dispatch(request, *args, **kwargs)
```

#### **Usage in Derived Views**
```python
class MySecureView(CustomBaseView):
    def get(self, request):
        return JsonResponse({"message": "Welcome!"})
```

#### **Custom CBV Table**  

| Custom CBV | Purpose |
|------------|---------|
| `CustomBaseView` | Ensures authentication before processing requests. |
| `LoggingMixin` | Logs every request made to the view. |
| `JsonResponseMixin` | Returns JSON responses instead of HTML. |

---

### **Asynchronous Views (Django 3.1+)**  

Django 3.1+ supports async views for better concurrency.

#### **Async View Example**  
```python
import asyncio
from django.http import JsonResponse

async def async_view(request):
    await asyncio.sleep(2)  # Simulate async task
    return JsonResponse({"message": "Async Response"})
```

#### **Async CBV Example**  
```python
from django.views import View

class AsyncCBV(View):
    async def get(self, request):
        await asyncio.sleep(2)
        return JsonResponse({"message": "Async CBV Response"})
```

---

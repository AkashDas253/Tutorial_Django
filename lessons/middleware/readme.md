## Django Middleware

---

### 🧠 Concept

**Middleware** in Django is a framework of hooks into Django’s request/response processing. It is a **lightweight, low-level plugin system** for globally altering input/output of the application. Each middleware component is responsible for performing **some specific function** during the processing of requests and/or responses.

Middleware sits between:
- the **request** and the **view**, and
- the **view’s response** and the **final response sent to the client**

---

### 🏗️ How Middleware Works Internally

The request/response cycle with middleware follows this flow:

```text
Client --> Request --> Middleware (IN) --> View --> Middleware (OUT) --> Response --> Client
```

Each middleware class can process:
- The request **before the view** is called
- The response **after the view** is called
- Any **exceptions** raised during view execution

Django wraps middleware classes **in order for requests** and **in reverse order for responses**.

---

### 📦 Middleware Lifecycle Methods

| Method                  | Trigger                             | Use Case Example                      |
|-------------------------|--------------------------------------|----------------------------------------|
| `__init__(self, get_response)` | On server startup         | Setup once-per-process                 |
| `__call__(self, request)`      | On every request           | Middleware chain entry point           |
| `process_request(self, request)` | Before view               | Modify request or block access         |
| `process_view(self, request, view_func, view_args, view_kwargs)` | Just before view | Conditional logic or logging           |
| `process_exception(self, request, exception)` | When view raises error | Handle or log exceptions               |
| `process_template_response(self, request, response)` | If response has `render()` | Modify before template rendering       |
| `process_response(self, request, response)` | After view             | Modify outgoing response               |

> ✅ Only `__init__` and `__call__` are required in modern Django (>=1.10+). Others are legacy and optional.

---

### 🧾 Example of Custom Middleware

```python
# myapp/middleware.py
class SimpleMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        print("Before view")
        response = self.get_response(request)
        print("After view")
        return response
```

---

### ⚙️ Adding Middleware

Add to `MIDDLEWARE` in `settings.py`:

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'myapp.middleware.SimpleMiddleware',
    ...
]
```

---

### 🧰 Built-in Middleware Examples

| Middleware                              | Purpose                                     |
|-----------------------------------------|---------------------------------------------|
| `SecurityMiddleware`                    | Adds headers like HSTS, XSS protection      |
| `SessionMiddleware`                     | Enables session support                     |
| `AuthenticationMiddleware`              | Attaches user to request                    |
| `CsrfViewMiddleware`                    | CSRF protection                             |
| `CommonMiddleware`                      | Handles redirects, trailing slashes         |
| `MessageMiddleware`                     | Enables Django's messaging framework        |
| `LocaleMiddleware`                      | Enables localization and translations       |

---

### 🧪 Use Cases

| Scenario                      | Solution                                  |
|-------------------------------|-------------------------------------------|
| Block IPs or rate-limit users | Custom middleware with IP checks          |
| Add custom headers            | Use `process_response` to attach headers  |
| Measure request time          | Track time in `__call__` or `process_view`|
| Global logging or auditing    | Log requests/responses in middleware      |
| Inject data to requests       | Attach values to `request` object         |

---

### 🚫 Common Mistakes

| Mistake                           | Impact                                        |
|----------------------------------|-----------------------------------------------|
| Long/blocking operations         | Delays every request                          |
| Modifying request improperly     | Breaks downstream views or middleware         |
| Forgetting to add in `MIDDLEWARE`| Middleware will not run                       |
| Using old-style middleware in modern Django | Causes unexpected behavior         |

---

### 📌 Summary

| Feature          | Description                                         |
|------------------|-----------------------------------------------------|
| Purpose          | Intercept/alter requests and responses globally     |
| Placement        | Set in `MIDDLEWARE` setting in order of execution  |
| Key Methods      | `__call__`, `process_request`, `process_response`, etc. |
| Best Use Cases   | Logging, security headers, timing, custom behavior |
| Modern Style     | Use class-based middleware with `__call__` and `get_response` |

---
---

## 🧩 Types of Django Middleware

Django does not define strict "types" of middleware classes, but based on **functionality** and **stage of execution**, we can categorize middleware into the following logical types:

---

### 🌐 1. **Request Middleware**
**Purpose**: Modify or inspect the HTTP request before it reaches the view.

| Common Methods | `__call__`, `process_request`, `process_view` |
| Use Cases |
|------------|
| IP blocking / rate limiting |
| Input sanitization |
| Logging requests |
| Injecting values into `request` object |

---

### 📤 2. **Response Middleware**
**Purpose**: Modify the response after the view has been executed.

| Common Methods | `process_response`, `__call__` |
| Use Cases |
|------------|
| Adding custom HTTP headers |
| Compressing response |
| Logging response times |
| Content filtering or modification |

---

### ❌ 3. **Exception Middleware**
**Purpose**: Handle exceptions raised during view execution.

| Common Method | `process_exception` |
| Use Cases |
|------------|
| Global error handling |
| Logging and alerting |
| Custom error messages |
| Silencing specific errors |

---

### 🧾 4. **View Middleware**
**Purpose**: Modify behavior just before the view is called.

| Common Method | `process_view` |
| Use Cases |
|------------|
| Conditional execution of views |
| Access control (e.g., role-based) |
| Feature flag handling |

---

### 🖼️ 5. **Template Response Middleware**
**Purpose**: Modify template responses before rendering.

| Common Method | `process_template_response` |
| Use Cases |
|------------|
| Inserting global context data |
| Modifying `TemplateResponse` object |
| Changing template name dynamically |

---

### ⚙️ 6. **Hybrid (General Purpose) Middleware**
**Purpose**: Implements a combination of the above.

| Common Methods | `__call__`, `process_request`, `process_response`, `process_exception` |
| Use Cases |
|------------|
| Session management |
| CSRF protection |
| Authentication |
| Localization |

---

### 🧠 Best Practice

Instead of thinking in rigid types, identify middleware by **what stage** it operates on and **what it modifies**:
- Request Middleware → Before view
- Response Middleware → After view
- Exception Middleware → On error
- Template Middleware → Before rendering

---

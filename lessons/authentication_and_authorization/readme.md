## **Authentication and Authorization in Django**

Django provides a built-in system for managing users, groups, permissions, login/logout, and session-based access control. It ensures both **authentication** (verifying identity) and **authorization** (access control).

---

### **1. User Model**

Django's default `User` model (from `django.contrib.auth.models`) includes:

* `username`
* `password` (hashed)
* `email`
* `first_name`, `last_name`
* `is_active`, `is_staff`, `is_superuser`
* `last_login`, `date_joined`

---

### **2. Creating and Managing Users**

```python
from django.contrib.auth.models import User

# Create user
user = User.objects.create_user(username="john", password="secret123")

# Superuser
User.objects.create_superuser(username="admin", password="adminpass", email="admin@example.com")

# Update user info
user.email = "new@example.com"
user.set_password("newpass123")
user.save()
```

---

### **3. Login and Logout**

#### Login View:

```python
from django.contrib.auth import authenticate, login

def login_view(request):
    if request.method == "POST":
        user = authenticate(username="john", password="secret123")
        if user is not None:
            login(request, user)
```

#### Logout:

```python
from django.contrib.auth import logout

def logout_view(request):
    logout(request)
```

---

### **4. Authentication Views and URLs**

Django provides ready-to-use auth views:

| View                  | URL name                                  |
| --------------------- | ----------------------------------------- |
| Login                 | `django.contrib.auth.views.LoginView`     |
| Logout                | `LogoutView`                              |
| Password change/reset | `PasswordChangeView`, `PasswordResetView` |

**URLs:**

```python
from django.urls import path, include

urlpatterns = [
    path('accounts/', include('django.contrib.auth.urls')),
]
```

---

### **5. Accessing Logged-In User**

```python
request.user  # User object
request.user.is_authenticated  # True if logged in
```

---

### **6. Login Required**

#### Function-Based View

```python
from django.contrib.auth.decorators import login_required

@login_required
def dashboard(request):
    ...
```

#### Class-Based View

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class DashboardView(LoginRequiredMixin, View):
    ...
```

---

### **7. Permissions**

#### Built-in:

* `is_staff`: Can access admin
* `is_superuser`: Full permissions

#### Custom:

```python
from django.contrib.auth.models import Permission

user.user_permissions.add(Permission.objects.get(codename='can_publish'))
```

Check permission in views:

```python
request.user.has_perm('app_label.can_publish')
```

---

### **8. Groups**

Groups bundle permissions.

```python
from django.contrib.auth.models import Group

# Create
editors = Group.objects.create(name='Editors')

# Assign to user
user.groups.add(editors)
```

---

### **9. Custom User Model**

Best practice for extensibility.

In `models.py`:

```python
from django.contrib.auth.models import AbstractUser

class CustomUser(AbstractUser):
    phone_number = models.CharField(max_length=15)
```

In `settings.py`:

```python
AUTH_USER_MODEL = 'myapp.CustomUser'
```

---

### **10. Password Management**

```python
user.set_password('newpassword')
user.check_password('password')
```

---

### **11. Password Validators**

In `settings.py`:

```python
AUTH_PASSWORD_VALIDATORS = [
    {'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator'},
    {'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator'},
    ...
]
```

---

### **12. Middleware and Backend**

Authentication backend and session handling:

```python
AUTHENTICATION_BACKENDS = ['django.contrib.auth.backends.ModelBackend']
```

---

### **13. Permissions in Templates**

```html
{% if user.is_authenticated %}
    Welcome, {{ user.username }}!
{% endif %}

{% if perms.app_label.permission_codename %}
    <a href="/restricted/">Restricted Area</a>
{% endif %}
```

---

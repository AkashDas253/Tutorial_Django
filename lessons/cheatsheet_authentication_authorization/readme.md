### **Django Authentication & Authorization Cheatsheet**  

---

## **Authentication vs Authorization**  
| Concept | Purpose |
|---------|---------|
| **Authentication** | Verifies **who** the user is. |
| **Authorization** | Determines **what** the user can do. |

---

## **1. Authentication (User Login, Logout, Registration)**  

### **Default Authentication Middleware (`settings.py`)**  
```python
MIDDLEWARE = [
    'django.contrib.auth.middleware.AuthenticationMiddleware',
]
```

### **User Model (`django.contrib.auth.models.User`)**  

| Field | Description |
|-------|------------|
| `username` | Unique identifier for the user. |
| `email` | User's email address. |
| `password` | Hashed password. |
| `is_staff` | Can access Django Admin. |
| `is_superuser` | Has all permissions. |
| `is_active` | Determines if the user is active. |

### **User Registration (`views.py`)**  
```python
from django.contrib.auth.models import User
from django.shortcuts import render, redirect
from django.contrib.auth import login

def register(request):
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']
        user = User.objects.create_user(username=username, password=password)
        login(request, user)
        return redirect('home')
    return render(request, 'register.html')
```

### **User Login & Logout (`views.py`)**  
```python
from django.contrib.auth import authenticate, login, logout
from django.shortcuts import render, redirect

def user_login(request):
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']
        user = authenticate(request, username=username, password=password)
        if user:
            login(request, user)
            return redirect('home')
    return render(request, 'login.html')

def user_logout(request):
    logout(request)
    return redirect('login')
```

### **Login Template (`login.html`)**  
```html
<form method="post">
    {% csrf_token %}
    <input type="text" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <button type="submit">Login</button>
</form>
```

---

## **2. Authorization (Permissions & Groups)**  

### **Checking Authentication in Views (`views.py`)**  
```python
from django.contrib.auth.decorators import login_required

@login_required
def dashboard(request):
    return render(request, 'dashboard.html')
```

### **Restricting Views by User Roles (`views.py`)**  
```python
from django.contrib.auth.decorators import permission_required

@permission_required('app_name.view_modelname', raise_exception=True)
def view_data(request):
    return render(request, 'data.html')
```

### **Custom Permissions in Models (`models.py`)**  
```python
class Document(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()

    class Meta:
        permissions = [
            ("can_view_secret", "Can view secret documents"),
        ]
```

### **Assigning Permissions to Users**  
```python
from django.contrib.auth.models import User, Permission

user = User.objects.get(username='john')
permission = Permission.objects.get(codename='can_view_secret')
user.user_permissions.add(permission)
```

### **Using Groups for Role-Based Access**  
```python
from django.contrib.auth.models import Group

# Create a group
editors = Group.objects.create(name="Editors")

# Assign user to group
user.groups.add(editors)
```

### **Checking User Permissions in Templates (`base.html`)**  
```html
{% if user.is_authenticated %}
    <p>Welcome, {{ user.username }}</p>
    <a href="{% url 'logout' %}">Logout</a>
{% else %}
    <a href="{% url 'login' %}">Login</a>
{% endif %}

{% if perms.app_name.can_view_secret %}
    <p>You have permission to view secret content.</p>
{% endif %}
```

---

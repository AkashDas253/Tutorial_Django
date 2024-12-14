## Django Basics

---

### **1. What is Django?**
Django is a high-level **Python web framework** that enables the rapid development of secure and maintainable web applications. It emphasizes the **DRY (Don't Repeat Yourself)** principle and promotes **reusability** and **pluggability**.

---

### **2. Key Features of Django**
- **MVT Architecture** (Model-View-Template)
- Built-in **Admin Interface**
- Built-in **Authentication System**
- High Scalability and Security
- ORM (Object-Relational Mapping)
- Middleware support
- Built-in support for caching, sessions, and messaging

---

### **3. Django Project Structure**
When you create a Django project, it generates the following structure:

```
project_name/
│
├── project_name/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│
├── manage.py
```

---

### **4. Setting Up Django**

1. **Install Django**:
   ```bash
   pip install django
   ```

2. **Create a Project**:
   ```bash
   django-admin startproject project_name
   ```

3. **Run the Development Server**:
   ```bash
   python manage.py runserver
   ```

4. **Create an App**:
   ```bash
   python manage.py startapp app_name
   ```

---

### **5. MVT Architecture**
Django follows the **Model-View-Template** pattern:
- **Model**: Manages the data and database schema.
- **View**: Contains the business logic.
- **Template**: Handles the presentation (HTML, CSS).

---

### **6. Configuration in `settings.py`**
Key configurations:
- **Installed Apps**: Add your app here.
   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'app_name',  # Add your app here
   ]
   ```
- **Database** (Default is SQLite):
   ```python
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.sqlite3',
           'NAME': BASE_DIR / 'db.sqlite3',
       }
   }
   ```

---

### **7. Django Models**
Models define the structure of your database.

- **Syntax**:
   ```python
   from django.db import models

   class ModelName(models.Model):
       field_name = models.FieldType(arguments)
   ```

- **Example**:
   ```python
   class Blog(models.Model):
       title = models.CharField(max_length=100)
       content = models.TextField()
       published_date = models.DateTimeField(auto_now_add=True)
   ```

- **Migrate the Model**:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

---

### **8. Django Views**
Views define the logic for what data to send to the templates.

- **Function-Based View**:
   ```python
   from django.http import HttpResponse

   def home(request):
       return HttpResponse("Welcome to Django!")
   ```

- **Class-Based View**:
   ```python
   from django.views import View
   from django.http import HttpResponse

   class HomeView(View):
       def get(self, request):
           return HttpResponse("Welcome to Class-Based View!")
   ```

---

### **9. Django URLs**
URLs map HTTP requests to views.

- **Define URL Patterns**:
   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('', views.home, name='home'),
   ]
   ```

- Include in Project URLs:
   ```python
   from django.contrib import admin
   from django.urls import path, include

   urlpatterns = [
       path('admin/', admin.site.urls),
       path('', include('app_name.urls')),
   ]
   ```

---

### **10. Django Templates**
Templates are HTML files used to render data dynamically.

- **Basic Example**:
   ```html
   <!DOCTYPE html>
   <html>
   <head><title>{{ title }}</title></head>
   <body>
       <h1>{{ heading }}</h1>
   </body>
   </html>
   ```

- **Render Template in View**:
   ```python
   from django.shortcuts import render

   def home(request):
       return render(request, 'home.html', {'title': 'Home', 'heading': 'Welcome!'})
   ```

---

### **11. Admin Interface**
Django provides a built-in admin interface.

- **Register Models**:
   ```python
   from django.contrib import admin
   from .models import Blog

   admin.site.register(Blog)
   ```

- **Access Admin**:
   Create a superuser:
   ```bash
   python manage.py createsuperuser
   ```

---

### **12. Forms in Django**
Forms handle user input.

- **Define a Form**:
   ```python
   from django import forms

   class BlogForm(forms.Form):
       title = forms.CharField(max_length=100)
       content = forms.CharField(widget=forms.Textarea)
   ```

- **Use in a View**:
   ```python
   from .forms import BlogForm

   def create_blog(request):
       form = BlogForm()
       return render(request, 'create_blog.html', {'form': form})
   ```

---

### **13. Django ORM**
Django provides an ORM for database operations.

- **Create**:
   ```python
   blog = Blog.objects.create(title="My Blog", content="Content here...")
   ```

- **Read**:
   ```python
   blogs = Blog.objects.all()
   single_blog = Blog.objects.get(id=1)
   ```

- **Update**:
   ```python
   blog = Blog.objects.get(id=1)
   blog.title = "Updated Title"
   blog.save()
   ```

- **Delete**:
   ```python
   blog = Blog.objects.get(id=1)
   blog.delete()
   ```

---

### **14. Middleware**
Middleware are hooks for request and response processing.

- **Add Middleware**:
   ```python
   MIDDLEWARE = [
       'django.middleware.security.SecurityMiddleware',
       'django.contrib.sessions.middleware.SessionMiddleware',
       'django.middleware.common.CommonMiddleware',
       'django.middleware.csrf.CsrfViewMiddleware',
   ]
   ```

---

### **15. Static Files**
Static files include CSS, JavaScript, and images.

- **Configure in `settings.py`**:
   ```python
   STATIC_URL = '/static/'
   ```

- **Use in Templates**:
   ```html
   {% load static %}
   <img src="{% static 'images/example.jpg' %}" alt="Example">
   ```

---

### **16. Deploying Django**
- Use **Gunicorn** and **Nginx** for deployment.
- Configure **settings.py** for production:
   ```python
   DEBUG = False
   ALLOWED_HOSTS = ['yourdomain.com']
   ```

--- 

### **Summary**
This guide covers the essentials of Django: setting up a project, working with models, views, templates, URLs, admin, forms, and ORM. For a deeper dive, refer to the [official documentation](https://docs.djangoproject.com).
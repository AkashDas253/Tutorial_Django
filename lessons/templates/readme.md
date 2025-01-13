# Template

## Setting up

1. **Create a Django Project**:
   ```sh
   django-admin startproject myproject
   cd myproject
   ```

2. **Create a Django App**:
   ```sh
   python manage.py startapp myapp
   ```

3. **Add the App to Installed Apps**:
   In `myproject/settings.py`, add `'myapp'` to the `INSTALLED_APPS` list:
   ```python
   INSTALLED_APPS = [
       ...
       'myapp',
   ]
   ```

4. **Create a Templates Directory**:
   Create a directory named `templates` inside your app directory (`myapp/templates`).

5. **Configure Template Settings**:
   In `myproject/settings.py`, configure the `TEMPLATES` setting to include the `templates` directory:
   ```python
   TEMPLATES = [
       {
           'BACKEND': 'django.template.backends.django.DjangoTemplates',
           'DIRS': [BASE_DIR / 'myapp/templates'],
           'APP_DIRS': True,
           'OPTIONS': {
               'context_processors': [
                   'django.template.context_processors.debug',
                   'django.template.context_processors.request',
                   'django.contrib.auth.context_processors.auth',
                   'django.contrib.messages.context_processors.messages',
               ],
           },
       },
   ]
   ```

6. **Create a Template File**:
   Inside the `templates` directory, create an HTML file (e.g., `index.html`):
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Django App</title>
   </head>
   <body>
       <h1>Hello, world!</h1>
   </body>
   </html>
   ```

7. **Create a View to Render the Template**:
   In `myapp/views.py`, create a view function to render the template:
   ```python
   from django.shortcuts import render

   def index(request):
       return render(request, 'index.html')
   ```

8. **Map the View to a URL**:
   In `myapp/urls.py`, map a URL to the view function:
   ```python
   from django.urls import path
   from .views import index

   urlpatterns = [
       path('', index, name='index'),
   ]
   ```

   Ensure `myapp/urls.py` is included in the project's `urls.py`:
   ```python
   from django.contrib import admin
   from django.urls import path, include

   urlpatterns = [
       path('admin/', admin.site.urls),
       path('', include('myapp.urls')),
   ]
   ```

9. **Run the Development Server**:
   ```sh
   python manage.py runserver
   ```

10. **Access the Template**:
    Open a web browser and navigate to `http://127.0.0.1:8000/` to see the rendered template.

This setup configures the template system in Django, allowing you to render HTML templates in response to web requests.

## Template Operations

### Template Inheritance
- Allows templates to inherit from other templates.
- Promotes reusability and maintainability.

  ```html
  <!-- base.html -->
  <!DOCTYPE html>
  <html>
  <head>
      <title>{% block title %}My Site{% endblock %}</title>
  </head>
  <body>
      {% block content %}{% endblock %}
  </body>
  </html>
  ```

  ```html
  <!-- child.html -->
  {% extends "base.html" %}

  {% block title %}Home{% endblock %}

  {% block content %}
  <h1>Welcome to my site!</h1>
  {% endblock %}
  ```

### Static Files
- Manages and uses static files in templates.
- Uses `{% static %}` template tag to link static files.

  ```html
  <!-- template.html -->
  <!DOCTYPE html>
  <html>
  <head>
      <link rel="stylesheet" type="text/css" href="{% static 'css/style.css' %}">
  </head>
  <body>
      <img src="{% static 'images/logo.png' %}" alt="Logo">
  </body>
  </html>
  ```

## Template Syntaxes

### Template Variables
- Uses variables in templates.
- Accesses context data passed from views.

  ```html
  <!-- template.html -->
  <p>Hello, {{ user.username }}!</p>
  ```

### Template Conditional Flow
- Uses conditional statements in templates.
- Implements logic with `{% if %}`, `{% elif %}`, and `{% else %}`.

  ```html
  <!-- template.html -->
  {% if user.is_authenticated %}
      <p>Welcome, {{ user.username }}!</p>
  {% else %}
      <p>Please log in.</p>
  {% endif %}
  ```

### Template Loops
- Uses loops in templates.
- Iterates over lists or querysets with `{% for %}`.

  ```html
  <!-- template.html -->
  <ul>
  {% for item in item_list %}
      <li>{{ item.name }}</li>
  {% endfor %}
  </ul>
  ```

### Template Filters
- Uses built-in and custom filters in templates.
- Modifies variables with filters like `{{ variable|filter }}`.

  ```html
  <!-- template.html -->
  <p>{{ text|lower }}</p>
  <p>{{ date|date:"Y-m-d" }}</p>
  ```

### Template Tags
- Uses built-in and custom template tags.
- Adds logic and functionality with `{% tag %}`.

  ```html
  <!-- template.html -->
  {% load custom_tags %}
  {% custom_tag %}
  ```

### Template Comments
- Adds comments in templates.
- Uses `{# comment #}` for comments that won't be rendered.

  ```html
  <!-- template.html -->
  {# This is a comment #}
  <p>Content here.</p>
  ```

### Template Includes
- Includes other templates within a template.
- Uses `{% include 'template.html' %}` to reuse template parts.

  ```html
  <!-- base.html -->
  <!DOCTYPE html>
  <html>
  <head>
      <title>My Site</title>
  </head>
  <body>
      {% include 'header.html' %}
      <div>
          {% block content %}{% endblock %}
      </div>
      {% include 'footer.html' %}
  </body>
  </html>
  ```

### Template Context Processors
- Uses context processors to add common data to templates.
- Adds data like `request`, `user`, etc., to all templates.

  ```python
  # settings.py
  TEMPLATES = [
      {
          'BACKEND': 'django.template.backends.django.DjangoTemplates',
          'DIRS': [],
          'APP_DIRS': True,
          'OPTIONS': {
              'context_processors': [
                  'django.template.context_processors.debug',
                  'django.template.context_processors.request',
                  'django.contrib.auth.context_processors.auth',
                  'django.contrib.messages.context_processors.messages',
              ],
          },
      },
  ]
  ```


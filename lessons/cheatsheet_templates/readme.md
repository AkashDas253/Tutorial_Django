### **Django Templates Cheatsheet**  

#### **Template Structure**  
- HTML files stored in `templates/` directory.  
- Uses **Django Template Language (DTL)** for dynamic content.  

#### **Rendering a Template in Views**  
```python
from django.shortcuts import render

def my_view(request):
    return render(request, 'my_template.html', {'key': 'value'})
```

#### **Template Variables**  
- Use `{{ variable_name }}` to display variables.  
```html
<p>Hello, {{ name }}!</p>
```

#### **Template Filters**  
- Modify variables in templates using `|` (pipe).  

| Filter | Description |
|--------|------------|
| ``{{ text|lower }}`` | Converts to lowercase. |
| ``{{ text|upper }}`` | Converts to uppercase. |
| ``{{ text|title }}`` | Capitalizes each word. |
| ``{{ text|length }}`` | Gets string length. |
| ``{{ list|join:", " }}`` | Joins list with a separator. |
| ``{{ date|date:"Y-m-d" }}`` | Formats date. |
| ``{{ text|default:"No data" }}`` | Sets default value if empty. |

#### **Template Tags**  
| Tag | Usage |
|-----|-------|
| `{% if condition %} ... {% endif %}` | Conditional rendering. |
| `{% for item in list %} ... {% endfor %}` | Loop through list. |
| `{% block name %} ... {% endblock %}` | Block for template inheritance. |
| `{% extends "base.html" %}` | Extends a base template. |
| `{% include "file.html" %}` | Includes another template. |

#### **Conditionals**  
```html
{% if user.is_authenticated %}
    <p>Welcome, {{ user.username }}</p>
{% else %}
    <p>Please log in.</p>
{% endif %}
```

#### **Loops**  
```html
<ul>
    {% for item in items %}
        <li>{{ item }}</li>
    {% endfor %}
</ul>
```

#### **Template Inheritance**  
**Base Template (`base.html`)**  
```html
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

**Child Template (`child.html`)**  
```html
{% extends "base.html" %}

{% block title %}Child Page{% endblock %}

{% block content %}
    <p>This is the child template content.</p>
{% endblock %}
```

#### **Static Files (CSS, JS, Images)**  
1. Add `'django.contrib.staticfiles'` in `INSTALLED_APPS`.  
2. Store files in `static/` folder.  
3. Load static files in templates:  
```html
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
<img src="{% static 'images/logo.png' %}" alt="Logo">
```

#### **CSRF Token in Forms**  
```html
<form method="post">
    {% csrf_token %}
    <button type="submit">Submit</button>
</form>
```

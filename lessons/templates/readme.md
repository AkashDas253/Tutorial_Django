## **Templates in Django**

Templates define the structure and layout of HTML pages using Django’s template language. They enable separation of logic and presentation in the **MTV architecture**.

---

### **1. Purpose**

* Generate dynamic HTML content.
* Render data passed from views.
* Apply logic using template tags and filters.

---

### **2. Template Files**

* Located inside the `templates/` directory.
* Registered in `settings.py` under `TEMPLATES['DIRS']`.

Example directory structure:

```
project/
├── app/
│   └── templates/
│       └── app/
│           └── example.html
```

---

### **3. Rendering Templates**

Use the `render()` function in views:

```python
from django.shortcuts import render

def welcome(request):
    return render(request, 'app/example.html', {'user': 'John'})
```

---

### **4. Template Tags**

Control logic within templates.

#### **Common tags:**

```django
{% if user %}
  Hello, {{ user }}!
{% endif %}

{% for item in items %}
  <li>{{ item }}</li>
{% endfor %}
```

| Tag                | Purpose                    |
| ------------------ | -------------------------- |
| `{% if %}`         | Conditional statements     |
| `{% for %}`        | Loops                      |
| `{% include %}`    | Include other templates    |
| `{% block %}`      | Define overrideable blocks |
| `{% extends %}`    | Inherit from base template |
| `{% csrf_token %}` | CSRF protection token      |

---

### **5. Template Filters**

Modify variables inside templates.

```django
{{ name|upper }}
{{ date|date:"Y-m-d" }}
```

| Filter           | Description              |
| ---------------- | ------------------------ |
| `date`           | Format datetime values   |
| `length`         | Number of items          |
| `default`        | Fallback value           |
| `lower`, `upper` | Case conversion          |
| `truncatechars`  | Truncate to n characters |

---

### **6. Template Inheritance**

Promotes reusable layout structure.

#### **base.html**

```django
<!DOCTYPE html>
<html>
<head>
  <title>{% block title %}Site{% endblock %}</title>
</head>
<body>
  {% block content %}{% endblock %}
</body>
</html>
```

#### **child.html**

```django
{% extends 'base.html' %}

{% block title %}Home{% endblock %}
{% block content %}
  <h1>Welcome</h1>
{% endblock %}
```

---

### **7. Template Context**

Data passed from the view to the template as a dictionary:

```python
context = {'name': 'Alice'}
return render(request, 'page.html', context)
```

---

### **8. Template Context Processors**

Functions that inject common data into all templates.

Enabled in `TEMPLATES['OPTIONS']['context_processors']`.

Common ones:

* `django.template.context_processors.request`
* `django.contrib.auth.context_processors.auth`
* `django.contrib.messages.context_processors.messages`

---

### **9. Using Static Files in Templates**

```django
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
```

---

### **10. Using URL Template Tag**

```django
<a href="{% url 'home' %}">Home</a>
```

---

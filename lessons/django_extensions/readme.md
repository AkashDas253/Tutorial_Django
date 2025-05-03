## **Django Extensions**

**Django Extensions** is a third-party package that adds powerful utilities to ease development, testing, debugging, and database management in Django projects.

> Install via:

```bash
pip install django-extensions
```

Add to `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...
    'django_extensions',
]
```

---

### **1. Useful Extensions**

| Extension              | Description                                        |
| ---------------------- | -------------------------------------------------- |
| `shell_plus`           | Auto-imports models and settings into Django shell |
| `runserver_plus`       | Enhanced dev server with Werkzeug debugger         |
| `show_urls`            | Lists all URL patterns                             |
| `sqldiff`              | Detects mismatches between models and DB           |
| `graph_models`         | Generates model relationship diagrams              |
| `export_emails`        | Exports user emails from the database              |
| `clean_pyc`            | Removes `.pyc` files recursively                   |
| `create_template_tags` | Creates custom template tag structure              |
| `create_command`       | Creates skeleton for custom management commands    |
| `reset_db`             | Drops and recreates the database                   |
| `print_settings`       | Prints settings with overridden values highlighted |

---

### **2. Popular Commands**

#### **shell\_plus**

```bash
python manage.py shell_plus
```

* Automatically imports all models
* Supports IPython / bpython / ptpython (if installed)

#### **runserver\_plus**

```bash
python manage.py runserver_plus
```

* Integrated Werkzeug debugger
* Debugs even on 500 errors
* Supports SSL

#### **show\_urls**

```bash
python manage.py show_urls
```

* Shows all project URL routes in tabular form
* Supports method filtering and formatting options

#### **graph\_models**

```bash
python manage.py graph_models your_app -o myapp.png
```

* Requires Graphviz
* Generates visual representation of model relationships

---

### **3. Database Utilities**

* `sqldiff`: Detects discrepancies between models and schema.
* `reset_db`: Useful during development for fresh DB state.
* `generate_secret_key`: Creates a secure Django `SECRET_KEY`.

---

### **4. Shell Enhancements**

* Pre-imports all models
* Recognizes custom user models
* IPython/Bpython support

Set shell preferences in `settings.py`:

```python
SHELL_PLUS = "ipython"
```

---

### **5. Custom Command Generators**

* `create_command <command_name>`: Sets up structure for custom `manage.py` commands.
* `create_template_tags <app_name>`: Creates `templatetags` module inside the app.

---

### **6. Email Export**

```bash
python manage.py export_emails --domain=example.com > emails.txt
```

Exports all user emails filtered by domain.

---

### **7. Debugging Tips**

* Combine with `django-debug-toolbar` for even deeper insights.
* Use `runserver_plus` for full traceback in browser.

---

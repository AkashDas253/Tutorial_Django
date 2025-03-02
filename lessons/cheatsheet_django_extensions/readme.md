### **Django Extensions Cheatsheet**  

Django Extensions is a package that provides additional management commands and utilities to enhance Django development.

---

## **1. Installation**  
```sh
pip install django-extensions
```

### **Add to `INSTALLED_APPS` (`settings.py`)**  
```python
INSTALLED_APPS = [
    ...,
    "django_extensions",
]
```

---

## **2. Useful Management Commands**  

| **Command** | **Description** |
|------------|----------------|
| `python manage.py shell_plus` | Enhanced shell with auto imports. |
| `python manage.py runscript script_name` | Runs Python scripts inside Django. |
| `python manage.py graph_models app_name -o models.png` | Generates model relationship diagrams. |
| `python manage.py show_urls` | Lists all project URLs. |
| `python manage.py create_superuser_plus` | Creates a superuser with additional fields. |
| `python manage.py sqldiff app_name` | Shows model vs. database differences. |
| `python manage.py clean_pyc` | Removes all `.pyc` files. |
| `python manage.py reset_db` | Drops and recreates the database. |

---

## **3. `shell_plus` - Interactive Shell with Auto Imports**  

### **Start Shell**
```sh
python manage.py shell_plus
```

### **Enable IPython or Jupyter Notebook** (`settings.py`)
```python
SHELL_PLUS = "ipython"
# or
SHELL_PLUS = "notebook"
```

---

## **4. Running Scripts**  

### **Create a Script in `scripts/` Directory**
```sh
python manage.py runscript my_script
```

### **Example Script (`scripts/my_script.py`)**
```python
from myapp.models import Book

def run():
    books = Book.objects.all()
    for book in books:
        print(book.title)
```

---

## **5. Generating Model Graphs**  

### **Generate a Diagram**
```sh
python manage.py graph_models myapp -o models.png
```

| **Flag** | **Description** |
|---------|----------------|
| `-o models.png` | Outputs a PNG file. |
| `-g` | Includes graph relationships. |

---

## **6. Listing URLs**  
```sh
python manage.py show_urls
```
| **Column** | **Description** |
|-----------|----------------|
| URL Pattern | URL route. |
| Name | Name of the URL. |
| View | Associated view function. |

---

## **7. Reset Database**  
```sh
python manage.py reset_db
```
⚠ **Warning:** This deletes all data.

---

## **8. Enhanced Superuser Creation**  
```sh
python manage.py create_superuser_plus
```
Supports additional fields like email and phone.

---

## **9. SQL Diff - Compare Models & Database**  
```sh
python manage.py sqldiff myapp
```
Shows discrepancies between models and the actual database.

---

## **10. Cleaning Bytecode Files**  
```sh
python manage.py clean_pyc
```
Removes `.pyc` and `__pycache__` files.

---

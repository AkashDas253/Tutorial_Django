### **Django Project Structure Cheatsheet**  

#### **Default Django Project Structure**  
```plaintext
my_project/
│── manage.py
│── db.sqlite3
│── my_project/  # Project package
│   │── __init__.py
│   │── settings.py
│   │── urls.py
│   │── asgi.py
│   │── wsgi.py
│── my_app/  # App package
│   │── __init__.py
│   │── admin.py
│   │── apps.py
│   │── models.py
│   │── views.py
│   │── urls.py
│   │── tests.py
│   │── forms.py
│   │── serializers.py (optional, for Django REST Framework)
│   └── migrations/
│── static/  # Static files (CSS, JS, images)
│── templates/  # HTML templates
└── requirements.txt  # Dependencies
```

#### **Key Files & Their Roles**  

| File/Folder | Purpose |
|-------------|---------|
| `manage.py` | CLI tool for managing the project (migrations, server, etc.). |
| `db.sqlite3` | Default SQLite database (if used). |
| `my_project/` | Main project folder (same name as project). |
| `settings.py` | Project settings (database, installed apps, middleware, etc.). |
| `urls.py` | URL routing for the project. |
| `asgi.py / wsgi.py` | ASGI/WSGI entry points for deploying Django apps. |
| `my_app/` | Individual Django app folder. |
| `models.py` | Defines database models. |
| `views.py` | Handles requests and responses. |
| `urls.py` | URL patterns specific to the app. |
| `admin.py` | Configuration for Django Admin. |
| `migrations/` | Stores database migration files. |
| `templates/` | HTML templates for rendering views. |
| `static/` | Stores CSS, JavaScript, images, etc. |
| `forms.py` | Defines forms using Django’s form API. |
| `tests.py` | Unit tests for the app. |
| `serializers.py` | Converts models to JSON (for APIs, optional). |
| `requirements.txt` | List of dependencies. |

#### **Running the Project**  
```sh
python manage.py runserver  # Start development server
```

## **Django Concepts and Subconcepts**

### **Basics**

* Django Project vs App
* MTV Architecture

  * Model
  * Template
  * View
* Request-Response Cycle
* WSGI/ASGI

### **Project Setup**

* `django-admin` and `manage.py`
* Project structure

  * `settings.py`
  * `urls.py`
  * `wsgi.py` / `asgi.py`
  * `__init__.py`

### **Settings**

* `INSTALLED_APPS`
* `MIDDLEWARE`
* `DATABASES`
* `TEMPLATES`
* `STATIC_URL`, `MEDIA_URL`
* `ALLOWED_HOSTS`, `DEBUG`, `SECRET_KEY`
* Environment Variables

### **URL Dispatcher**

* `urls.py` in project and apps
* `path()`, `re_path()`, `include()`
* Named URLs
* URL Parameters

### **Views**

* Function-based Views (FBV)
* Class-based Views (CBV)

  * `View`, `TemplateView`, `ListView`, `DetailView`, `CreateView`, `UpdateView`, `DeleteView`
* Mixins
* `HttpResponse`, `JsonResponse`
* Redirects and Status Codes

### **Templates**

* Template Language

  * Tags: `{% for %}`, `{% if %}`, `{% include %}`, `{% extends %}`, `{% block %}`
  * Filters: `{{ name|upper }}`, `{{ date|date:"Y-m-d" }}`
* Template Inheritance
* Context Dictionary
* Static Files (`{% static %}`)
* Template Loaders

### **Models**

* Model Definition

  * Fields: `CharField`, `IntegerField`, `BooleanField`, `DateTimeField`, `TextField`, `FileField`, etc.
  * Meta options
  * `__str__()` method
* Relationships

  * `ForeignKey`
  * `OneToOneField`
  * `ManyToManyField`
* Model Managers and QuerySets
* Model Methods
* Model Inheritance

### **Migrations**

* `makemigrations`, `migrate`
* `showmigrations`
* Custom Migrations
* Schema Evolution

### **Admin Interface**

* Register Models
* Customize Admin

  * `list_display`, `search_fields`, `list_filter`
  * Inline Models
* Admin Actions

### **Forms**

* `Form` and `ModelForm`
* Validation

  * `clean()`, `clean_<field>()`
  * Custom Validators
* Widgets
* CSRF Protection
* Error Handling

### **ORM (Object-Relational Mapping)**

* CRUD Operations
* Filtering, Excluding
* Aggregation and Annotation
* Ordering and Limiting
* `Q` Objects and Complex Queries
* Raw SQL Queries
* Transactions

### **Authentication and Authorization**

* User Model (`AbstractUser`, `AbstractBaseUser`)
* `login`, `logout`, `authenticate`
* Permissions and Groups
* `@login_required`
* `PermissionRequiredMixin`
* Custom User Models

### **Sessions and Cookies**

* Session Backends
* Setting and Getting Cookies
* Session Expiry
* Secure Cookies

### **Middleware**

* Custom Middleware
* Built-in Middleware
* Request and Response Processing

### **Signals**

* Built-in Signals: `post_save`, `pre_delete`
* Connecting and Disconnecting Signals
* Custom Signals

### **Testing**

* `TestCase`, `Client`
* Unit Tests for Views, Models, Forms
* Fixtures
* `pytest-django` (optional integration)

### **Static and Media Files**

* `STATICFILES_DIRS`, `STATIC_ROOT`
* `MEDIA_ROOT`, `MEDIA_URL`
* `collectstatic`

### **Deployment**

* Production Settings

  * Debug = False
  * Allowed Hosts
  * Secure Cookies
* Using WSGI/ASGI
* Reverse Proxy Setup
* Static and Media Files Setup

### **Security**

* CSRF, XSS, SQL Injection Protection
* Secure Password Hashing
* HTTPS settings
* `SECURE_*` settings

### **Internationalization (i18n) and Localization (l10n)**

* `gettext`, `ugettext_lazy`
* Translation Files (`.po`, `.mo`)
* Time Zone Support
* Locale Middleware

### **REST Framework (Django REST Framework - DRF)**

* Serializers

  * `Serializer`, `ModelSerializer`
* API Views

  * `APIView`, `GenericAPIView`, ViewSets
* Routers
* Authentication

  * TokenAuth, SessionAuth, JWT
* Permissions
* Throttling and Pagination
* Browsable API

### **Advanced Topics**

* Caching

  * Per-view, Per-site, Low-level
* Celery Integration (Async Tasks)
* Channels (WebSockets)
* Custom Management Commands
* ContentTypes Framework
* Generic Relations

### **Third-party Integrations**

* Django Allauth (Authentication)
* Django Debug Toolbar
* Django Extensions
* Social Authentication
* Storage Backends (AWS S3, etc.)

---

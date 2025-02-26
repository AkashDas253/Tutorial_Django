# Project and Application

## Setup 

### Installing Django

```bash
# Activate virtual environment
pip install django # Install Django
```

### Create Project
```bash
# Using django admin
django-admin startproject project_name # New Project directory
django-admin startproject project_name . # Project in the same directory
```

### Create Application and add to Project
```bash
# Inside the project folder
python manage.py startapp app_name
```

### Add Application to Projects
Add the Application to `INSTALLED_APPS` in `settings.py`:
```python
# project/settings.py
INSTALLED_APPS = [
    ...,
    'app_name',
]
```

## Structure

```plaintext
project_name/
├── manage.py
├── project6/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
└── app_name/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    └── views.py
```

### Working of Each File

- **manage.py**: A command-line utility that lets you interact with this Django project. You can use it to run the development server, create apps, and perform other administrative tasks.

- **project_name/**: The inner directory with the same name as your project. This is the actual Python package for your project.
  - **__init__.py**: An empty file that tells Python that this directory should be considered a Python package.
  - **settings.py**: Contains all the settings/configuration for your Django project.

    - **Middleware**: Middleware components are used to process requests and responses globally in Django. They can be used for various purposes such as authentication, session management, and caching.

    - **Database Configuration**: Django supports multiple databases, and the `DATABASES` setting is used to configure the connection details for each database.

    - **Static Files**: The `STATIC_URL` setting specifies the URL prefix for static files (e.g., CSS, JavaScript) served by Django. The `STATIC_ROOT` setting defines the directory where static files are collected during deployment.

    - **Templates**: The `TEMPLATES` setting is used to configure the template engine in Django. It includes options such as the template directories, context processors, and template loaders.

    - **Internationalization**: Django provides built-in support for internationalization (i18n) and localization (l10n). The `LANGUAGE_CODE` setting defines the default language for the project, and the `LOCALE_PATHS` setting specifies the directories containing translation files.

    - **Timezone**: The `TIME_ZONE` setting determines the default timezone used by the project.

    - **Installed Apps**: The `INSTALLED_APPS` setting lists all the Django apps installed in the project. Each app can define models, views, and other components.

    - **Authentication**: Django provides a built-in authentication system. The `AUTHENTICATION_BACKENDS` setting defines the authentication backends used for user authentication.

    - **Logging**: The `LOGGING` setting is used to configure the logging behavior in Django. It includes options for defining loggers, handlers, and log levels.

    - **Caching**: Django supports caching to improve performance. The `CACHES` setting is used to configure the caching backend and its options.

    - **Email**: The `EMAIL_BACKEND` setting specifies the email backend used for sending emails in Django.

    - **Security**: Django includes various security settings, such as `SECRET_KEY` for cryptographic signing, `CSRF_COOKIE_SECURE` to enforce secure CSRF cookies, and `SESSION_COOKIE_SECURE` to enforce secure session cookies.

    - **Debugging**: The `DEBUG` setting determines whether Django runs in debug mode, which provides detailed error messages during development.

    - **Media Files**: The `MEDIA_URL` setting specifies the URL prefix for media files (e.g., user-uploaded files) served by Django. The `MEDIA_ROOT` setting defines the directory where media files are stored.

    - **Middleware Classes**: The `MIDDLEWARE` setting lists the middleware classes to be used in the project. The order of middleware classes determines the order in which they are applied to requests and responses.

    - **Allowed Hosts**: The `ALLOWED_HOSTS` setting specifies the valid hostnames that the project can serve.

    - **CSRF Protection**: The `CSRF_COOKIE_HTTPONLY` setting determines whether the CSRF cookie is accessible by JavaScript.

    - **Session Configuration**: The `SESSION_COOKIE_NAME` setting specifies the name of the session cookie, and the `SESSION_COOKIE_SECURE` setting determines whether the session cookie is secure.

    - **File Uploads**: The `FILE_UPLOAD_MAX_MEMORY_SIZE` setting defines the maximum size of files that can be uploaded.

    - **Pagination**: The `REST_FRAMEWORK` setting is used to configure pagination options for Django REST Framework.

    - **Cache Control**: The `CACHE_MIDDLEWARE_SECONDS` setting specifies the maximum age of cached pages.

    - **Cross-Origin Resource Sharing (CORS)**: The `CORS_ORIGIN_ALLOW_ALL` setting determines whether the project allows cross-origin requests from any origin.

    - **Database Caching**: The `DATABASE_CACHE_ALIAS` setting specifies the database alias to use for database caching.

    - **HTTPS Redirect**: The `SECURE_PROXY_SSL_HEADER` setting is used to redirect HTTP requests to HTTPS.

    - **File Storage**: The `DEFAULT_FILE_STORAGE` setting specifies the default storage backend for file uploads.

    - **Email Backend**: The `EMAIL_BACKEND` setting determines the backend used for sending emails.

    - **URL Configuration**: The `ROOT_URLCONF` setting specifies the Python module containing the project's URL patterns.

    - **CSRF Middleware**: The `CSRF_COOKIE_SECURE` setting determines whether the CSRF cookie is secure.

    - **Session Middleware**: The `SESSION_COOKIE_SECURE` setting determines whether the session cookie is secure.

    - **Password Validation**: The `AUTH_PASSWORD_VALIDATORS` setting lists the validators used for password validation.

    - **CSRF Middleware**: The `CSRF_COOKIE_HTTPONLY` setting determines whether the CSRF cookie is accessible by JavaScript.

    - **Session Middleware**: The `SESSION_COOKIE_HTTPONLY` setting determines whether the session cookie is accessible by JavaScript.

    - **CSRF Middleware**: The `CSRF_COOKIE_SAMESITE` setting determines the SameSite attribute of the CSRF cookie.

    - **Session Middleware**: The `SESSION_COOKIE_SAMESITE` setting determines the SameSite attribute of the session cookie.

  - **urls.py**: The URL declarations for this Django project; a "table of contents" of your Django-powered site.
  - **asgi.py**: An entry-point for ASGI-compatible web servers to serve your project. ASGI is the asynchronous successor to WSGI.
  - **wsgi.py**: An entry-point for WSGI-compatible web servers to serve your project. WSGI is the standard for Python web application deployment.

- **app_name/**: A directory for a Django app within your project. Replace `app_name` with the actual name of your app.
  - **__init__.py**: An empty file that tells Python that this directory should be considered a Python package.
  - **admin.py**: Configuration for the Django admin interface.
  - **apps.py**: Configuration for the app itself.
  - **models.py**: Defines the data models for the app.
  - **tests.py**: Contains test cases for the app.
  - **views.py**: Contains the views for the app, which handle the logic for rendering web pages.


## Features of Django and Its Components

### Key Features of Django

1. **Admin Interface**:
   - Automatically generated admin interface for managing application data.
   - Customizable and extendable.

2. **ORM (Object-Relational Mapping)**:
   - Allows interaction with the database using Python objects.
   - Supports multiple databases (e.g., PostgreSQL, MySQL, SQLite).

3. **MTV Architecture (Model-Template-View)**:
   - Separates data (Model), user interface (Template), and business logic (View).

4. **URL Routing**:
   - Maps URLs to views using a URL dispatcher.
   - Supports clean and readable URLs.

5. **Form Handling**:
   - Simplifies form creation, validation, and processing.
   - Supports both HTML forms and Django forms.

6. **Authentication and Authorization**:
   - Built-in user authentication system.
   - Supports user registration, login, logout, password management, and permissions.

7. **Middleware**:
   - Hooks into Django's request/response processing.
   - Can be used for session management, authentication, etc.

8. **Security**:
   - Protects against common web vulnerabilities (e.g., XSS, CSRF, SQL injection).
   - Provides secure password hashing and user authentication.

9. **Scalability**:
   - Designed to handle high-traffic websites.
   - Supports caching, load balancing, and database optimization.

10. **Internationalization**:
    - Supports translation of text and formatting of dates, times, and numbers.

### Components of Django and Their Working

1. **Models**:
   - Define the structure of the database.
   - Represent tables in the database.

2. **Views**:
   - Handle the logic for processing requests and returning responses.
   - Can return HTML, JSON, or other formats.

3. **Templates**:
   - Define the HTML structure of the web pages.
   - Use Django Template Language (DTL) for dynamic content.

4. **URLs**:
   - Map URLs to views.
   - Use regular expressions or path converters.

5. **Forms**:
   - Simplify form creation and validation.
   - Can be used for both HTML forms and Django forms.

6. **Admin**:
   - Provides an interface for managing application data.
   - Customizable and extendable.

7. **Middleware**:
   - Hooks into Django's request/response processing.
   - Can be used for session management, authentication, etc.

8. **Settings**:
   - Configuration for the Django project.
   - Includes database settings, installed apps, middleware, etc.


## **`django` Modules and Submodules:**

- **`# Use django.conf`**
  - `django.conf.settings` # Access Django project settings.
  - `django.conf.urls` # URL configuration for routing.

- **`# Use django.db`**
  - `django.db.models.Model` # Base class for Django models.
  - `django.db.models.CharField` # Character field for model fields.
  - `django.db.models.IntegerField` # Integer field for model fields.
  - `django.db.models.ForeignKey` # Foreign key field for relationships between models.
  - `django.db.models.OneToOneField` # One-to-one relationship field.
  - `django.db.models.ManyToManyField` # Many-to-many relationship field.
  - `django.db.models.DateTimeField` # Date-time field for models.
  - `django.db.models.QuerySet` # Represents a set of model instances.
  - `django.db.models.Manager` # Custom manager for model querysets.

- **`# Use django.forms`**
  - `django.forms.Form` # Base class for Django forms.
  - `django.forms.ModelForm` # Form class for creating models from forms.
  - `django.forms.CharField` # Character field for forms.
  - `django.forms.IntegerField` # Integer field for forms.
  - `django.forms.EmailField` # Email field for forms.
  - `django.forms.DateField` # Date field for forms.
  - `django.forms.BooleanField` # Boolean field for forms.
  - `django.forms.FileField` # File field for forms.

- **`# Use django.views`**
  - `django.views.View` # Base view class for handling HTTP requests.
  - `django.views.generic.TemplateView` # Render templates in views.
  - `django.views.generic.ListView` # Display a list of model instances.
  - `django.views.generic.DetailView` # Display details for a single model instance.
  - `django.views.generic.CreateView` # Display a form for creating a new model instance.
  - `django.views.generic.UpdateView` # Display a form for updating a model instance.
  - `django.views.generic.DeleteView` # Display a confirmation form to delete a model instance.

- **`# Use django.urls`**
  - `django.urls.path` # Define URL patterns.
  - `django.urls.re_path` # Define URL patterns with regular expressions.
  - `django.urls.include` # Include URL patterns from another module.

- **`# Use django.middleware`**
  - `django.middleware.common.CommonMiddleware` # Middleware to handle common HTTP operations.
  - `django.middleware.security.SecurityMiddleware` # Middleware for managing security settings.
  - `django.middleware.csrf.CsrfViewMiddleware` # Middleware for CSRF protection.
  - `django.middleware.gzip.GZipMiddleware` # Middleware to handle GZip compression.
  - `django.middleware.transaction.TransactionMiddleware` # Middleware for handling database transactions.

- **`# Use django.contrib`**
  - `django.contrib.admin` # Admin interface for managing models and data.
    - `django.contrib.admin.ModelAdmin` # Custom configuration for model management in the admin interface.
    - `django.contrib.admin.site.register` # Register models with the admin interface.
  - `django.contrib.auth` # User authentication and authorization system.
    - `django.contrib.auth.models.User` # Default User model.
    - `django.contrib.auth.forms.UserCreationForm` # Form for creating a new user.
    - `django.contrib.auth.forms.AuthenticationForm` # Form for authenticating a user.
    - `django.contrib.auth.views.LoginView` # Built-in login view.
    - `django.contrib.auth.views.LogoutView` # Built-in logout view.
  - `django.contrib.sessions` # Session management system.
  - `django.contrib.messages` # Framework for temporary messages in views.
  - `django.contrib.staticfiles` # Static files management.

- **`# Use django.template`**
  - `django.template.Context` # Represents the context for rendering templates.
  - `django.template.loader.get_template` # Load a template from a file.
  - `django.template.loader.render_to_string` # Render a template to a string.

- **`# Use django.http`**
  - `django.http.HttpRequest` # Represents an HTTP request.
  - `django.http.HttpResponse` # Represents an HTTP response.
  - `django.http.JsonResponse` # Represents a JSON response.
  - `django.http.Http404` # Exception for raising 404 errors.
  - `django.http.HttpResponseRedirect` # Redirects to a new URL.

- **`# Use django.contrib.sites`**
  - `django.contrib.sites.models.Site` # Model representing a website.
  - `django.contrib.sites.shortcuts.get_current_site` # Get the current site from the request.

- **`# Use django.db.migrations`**
  - `django.db.migrations.Migration` # Base class for creating migration files.
  - `django.db.migrations.RunPython` # Run custom Python code during migrations.
  - `django.db.migrations.RunSQL` # Run raw SQL commands during migrations.
  - `django.db.migrations.AlterField` # Alter a model field in a migration.

- **`# Use django.contrib.syndication`**
  - `django.contrib.syndication.views.Feed` # Base class for generating feeds (RSS, Atom).

- **`# Use django.contrib.markup`**
  - `django.contrib.markup.templatetags.markup` # Provides filters for rendering markdown in templates.

- **`# Use django.test`**
  - `django.test.TestCase` # Base class for creating tests for Django applications.
  - `django.test.Client` # A test client for simulating requests to the Django application.

- **`# Use django.db.backends`**
  - `django.db.backends.postgresql` # PostgreSQL database backend.
  - `django.db.backends.mysql` # MySQL database backend.
  - `django.db.backends.sqlite3` # SQLite database backend.

- **`# Use django.db.models.query`**
  - `django.db.models.query.QuerySet` # Represents a collection of model instances.
  - `django.db.models.query.F` # Represents a field reference in queries.

- **`# Use django.db.models.signals`**
  - `django.db.models.signals.pre_save` # Signal sent before a model is saved.
  - `django.db.models.signals.post_save` # Signal sent after a model is saved.
  - `django.db.models.signals.pre_delete` # Signal sent before a model is deleted.
  - `django.db.models.signals.post_delete` # Signal sent after a model is deleted.

- **`# Use django.db.utils`**
  - `django.db.utils.ConnectionHandler` # Handles database connections.

- **`# Use django.db.transaction`**
  - `django.db.transaction.atomic` # Mark a block of code to be run within a database transaction.
  - `django.db.transaction.commit` # Commit a database transaction.
  - `django.db.transaction.rollback` # Rollback a database transaction.

- **`# Use django.views.decorators`**
  - `django.views.decorators.cache.cache_page` # Cache the output of a view.
  - `django.views.decorators.csrf.csrf_protect` # Protect a view with CSRF tokens.
  - `django.views.decorators.http.require_http_methods` # Require specific HTTP methods for a view.

- **`# Use django.core`**
  - `django.core.mail.send_mail` # Send an email.
  - `django.core.cache.cache` # Access the cache framework.
  - `django.core.management.call_command` # Call management commands programmatically.

- **`# Use django.contrib.auth.tokens`**
  - `django.contrib.auth.tokens.default_token_generator` # Default token generator for password reset.

---

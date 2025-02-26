## Setting up
- URL configurations in Django.

## URL Configuration

### Project-Level URLs
- Defines URL patterns in the project's `urls.py`.
- Routes URLs to views at the project level.

  ```python
  # project/urls.py
  from django.contrib import admin
  from django.urls import path, include

  urlpatterns = [
      path('admin/', admin.site.urls),
      path('app_name/', include('app_name.urls')),
  ]
  ```

### App-Level URLs
- Defines URL patterns in the app's `urls.py`.
- Routes URLs to views at the app level.

  ```python
  # app_name/urls.py
  from django.urls import path
  from . import views

  urlpatterns = [
      path('', views.index, name='index'),
  ]
  ```

### Including URLs
- Includes app-level URLs in the project-level `urls.py`.

  ```python
  # project/urls.py
  from django.contrib import admin
  from django.urls import path, include

  urlpatterns = [
      path('admin/', admin.site.urls),
      path('app_name/', include('app_name.urls')),
  ]
  ```
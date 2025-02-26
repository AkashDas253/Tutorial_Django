
## URL Patterns

### Path Function
- Uses the `path` function to define URL patterns.
- Simple string-based URL patterns.

  ```python
  # project/urls.py
  from django.urls import path
  from . import views

  urlpatterns = [
      path('', views.home, name='home'),
      path('about/', views.about, name='about'),
  ]
  ```

### Re_path Function
- Uses the `re_path` function for regex-based URL patterns.
- Allows complex URL matching using regular expressions.

  ```python
  # project/urls.py
  from django.urls import re_path
  from . import views

  urlpatterns = [
      re_path(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive, name='year_archive'),
  ]
  ```

### URL Parameters
- Captures URL parameters and passes them to views.
- Supports different types like `<int:id>`, `<str:name>`, etc.

  ```python
  # project/urls.py
  from django.urls import path
  from . import views

  urlpatterns = [
      path('article/<int:id>/', views.article_detail, name='article_detail'),
  ]
  ```

### Named URL Patterns
- Names URL patterns for easier reference.
- Useful for reverse URL resolution.

  ```python
  # project/urls.py
  from django.urls import path
  from . import views

  urlpatterns = [
      path('', views.home, name='home'),
      path('about/', views.about, name='about'),
  ]
  ```
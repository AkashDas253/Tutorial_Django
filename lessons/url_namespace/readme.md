
## URL Namespaces

### Using Namespaces
- Uses namespaces to organize URL names.
- Helps avoid URL name conflicts.

  ```python
  # project/urls.py
  from django.urls import path, include

  urlpatterns = [
      path('app_name/', include(('app_name.urls', 'app_name'), namespace='app_name')),
  ]
  ```

### Including Namespaces
- Includes namespaces in URL configurations.
- Defines `app_name` in the app's `urls.py`.

  ```python
  # app_name/urls.py
  from django.urls import path
  from . import views

  app_name = 'app_name'

  urlpatterns = [
      path('', views.index, name='index'),
  ]
  ```

## URL Reverse Resolution

### Using reverse()
- Uses the `reverse()` function to get URL paths from view names.
- Useful for generating URLs dynamically.

  ```python
  from django.urls import reverse

  def my_view(request):
      url = reverse('app_name:index')
      # use the url
  ```

### Using reverse_lazy()
- Uses the `reverse_lazy()` function for URL resolution.
- Useful in class-based views where URL resolution is needed before the view is fully initialized.

  ```python
  from django.urls import reverse_lazy
  from django.views.generic.edit import CreateView
  from .models import MyModel

  class MyCreateView(CreateView):
      model = MyModel
      success_url = reverse_lazy('app_name:index')
  ```
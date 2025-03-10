 ## **Django Class-Based Views (CBVs) List**

| Category | View Name | Description | Primary Methods |  
|----------|-----------|------------|-----------------|  
| **Base Views** | `View` | The simplest base view for handling HTTP requests. | `dispatch()`, `http_method_not_allowed()` |  
| | `TemplateView` | Renders an HTML template with optional context data. | `get_context_data()` |  
| | `RedirectView` | Redirects to another URL. | `get_redirect_url()`, `get()`, `post()` |  
| | `ContextMixin` | Provides additional context data. | `get_context_data()` |  
| **Generic Display Views** | `DetailView` | Displays a single model instance. | `get_object()`, `get_context_data()` |  
| | `ListView` | Displays multiple model instances in a list format. | `get_queryset()`, `paginate_queryset()` |  
| **Generic Editing Views** | `CreateView` | Handles object creation via a form. | `form_valid()`, `form_invalid()` |  
| | `UpdateView` | Manages updates to an existing object. | `get_object()`, `form_valid()` |  
| | `DeleteView` | Handles object deletion and redirects. | `get_object()`, `delete()` |  
| | `ModelFormMixin` | Adds model form support to views. | `get_form_class()`, `form_valid()` |  
| **Generic Form Handling Views** | `FormView` | Displays and processes an HTML form. | `get_form_class()`, `form_valid()`, `form_invalid()` |  
| | `FormMixin` | Adds form-handling capabilities to views. | `get_form_class()`, `get_success_url()` |  
| **Generic Date-Based Views** | `ArchiveIndexView` | Displays a list of objects sorted by date. | `get_dated_items()` |  
| | `YearArchiveView` | Displays objects from a specific year. | `get_year()` |  
| | `MonthArchiveView` | Displays objects from a specific month. | `get_month()` |  
| | `WeekArchiveView` | Displays objects from a specific week. | `get_week()` |  
| | `DayArchiveView` | Displays objects from a specific day. | `get_day()` |  
| | `TodayArchiveView` | Displays objects from the current day. | `get_today()` |  
| | `DateDetailView` | Displays a single object based on a date field. | `get_object()` |  
| **Generic Processing Views** | `ProcessFormView` | Handles form submissions without rendering a template. | `post()`, `form_valid()` |  
| | `SingleObjectMixin` | Provides object retrieval for CBVs. | `get_object()`, `get_queryset()` |  
| | `MultipleObjectMixin` | Provides queryset handling for CBVs. | `get_queryset()`, `paginate_queryset()` |  
| | `TemplateResponseMixin` | Controls rendering of templates in views. | `render_to_response()` |  
| **Authentication-Based Views** | `LoginView` | Handles user login. | `form_valid()`, `form_invalid()` |  
| | `LogoutView` | Logs out the user and redirects. | `get_next_page()` |  
| | `PasswordChangeView` | Allows users to change their password. | `form_valid()`, `form_invalid()` |  
| | `PasswordResetView` | Handles password reset requests. | `form_valid()`, `get_success_url()` |  
| | `PasswordResetConfirmView` | Confirms and sets a new password. | `form_valid()`, `form_invalid()` |  
| | `PasswordResetDoneView` | Displays success message after password reset email is sent. | `get()` |  
| | `PasswordResetCompleteView` | Final page after resetting password. | `get()` |  
| **Mixins Used with CBVs** | `LoginRequiredMixin` | Restricts access to authenticated users. |  
| | `PermissionRequiredMixin` | Ensures users have specific permissions. |  
| | `FormMixin` | Adds form-handling capabilities. |  
| | `ContextMixin` | Passes additional context data to templates. |  
| | `SingleObjectMixin` | Retrieves a single object for CBVs. |  
| | `MultipleObjectMixin` | Manages multiple objects in CBVs. |  
| | `SuccessMessageMixin` | Displays success messages after form submission. |  
| | `UserPassesTestMixin` | Restricts access based on a test function. |  
| | `AccessMixin` | Base class for authentication mixins. |  
| | `AjaxResponseMixin` | Handles AJAX responses in views. |  

---

---

## **Django Class-Based Views (CBVs) Overview**

Django provides a variety of **Class-Based Views (CBVs)** categorized based on their functionality. Below is a detailed tabular representation of all CBVs in Django.

### **1. Base Views**  
These views provide fundamental request handling and can be extended for custom implementations.

| View Name | Description | Primary Methods |
|-----------|------------|-----------------|
| `View` | The simplest base view for handling HTTP requests. | `dispatch()`, `http_method_not_allowed()` |
| `TemplateView` | Renders an HTML template with optional context data. | `get_context_data()` |
| `RedirectView` | Redirects to another URL. | `get_redirect_url()`, `get()`, `post()` |
| `ContextMixin` | Provides additional context data. | `get_context_data()` |

---

### **2. Generic Display Views**  
These views retrieve and display model objects without modification.

| View Name | Description | Primary Methods |
|-----------|------------|-----------------|
| `DetailView` | Displays a single model instance. | `get_object()`, `get_context_data()` |
| `ListView` | Displays multiple model instances in a list format. | `get_queryset()`, `paginate_queryset()` |

---

### **3. Generic Editing Views**  
These views allow CRUD (Create, Read, Update, Delete) operations on model objects.

| View Name | Description | Primary Methods |
|-----------|------------|-----------------|
| `CreateView` | Handles object creation via a form. | `form_valid()`, `form_invalid()` |
| `UpdateView` | Manages updates to an existing object. | `get_object()`, `form_valid()` |
| `DeleteView` | Handles object deletion and redirects. | `get_object()`, `delete()` |
| `ModelFormMixin` | Adds model form support to views. | `get_form_class()`, `form_valid()` |

---

### **4. Generic Form Handling Views**  
These views provide form processing capabilities.

| View Name | Description | Primary Methods |
|-----------|------------|-----------------|
| `FormView` | Displays and processes an HTML form. | `get_form_class()`, `form_valid()`, `form_invalid()` |
| `FormMixin` | Adds form-handling capabilities to views. | `get_form_class()`, `get_success_url()` |

---

### **5. Generic Date-Based Views**  
These views organize and display objects based on date fields.

| View Name | Description | Primary Methods |
|-----------|------------|-----------------|
| `ArchiveIndexView` | Displays a list of objects sorted by date. | `get_dated_items()` |
| `YearArchiveView` | Displays objects from a specific year. | `get_year()` |
| `MonthArchiveView` | Displays objects from a specific month. | `get_month()` |
| `WeekArchiveView` | Displays objects from a specific week. | `get_week()` |
| `DayArchiveView` | Displays objects from a specific day. | `get_day()` |
| `TodayArchiveView` | Displays objects from the current day. | `get_today()` |
| `DateDetailView` | Displays a single object based on a date field. | `get_object()` |

---

### **6. Generic Processing Views**  
These views handle additional tasks such as redirecting and HTTP methods.

| View Name | Description | Primary Methods |
|-----------|------------|-----------------|
| `ProcessFormView` | Handles form submissions without rendering a template. | `post()`, `form_valid()` |
| `SingleObjectMixin` | Provides object retrieval for CBVs. | `get_object()`, `get_queryset()` |
| `MultipleObjectMixin` | Provides queryset handling for CBVs. | `get_queryset()`, `paginate_queryset()` |
| `TemplateResponseMixin` | Controls rendering of templates in views. | `render_to_response()` |

---

### **7. Authentication-Based Views**  
These views handle login, logout, and password management.

| View Name | Description | Primary Methods |
|-----------|------------|-----------------|
| `LoginView` | Handles user login. | `form_valid()`, `form_invalid()` |
| `LogoutView` | Logs out the user and redirects. | `get_next_page()` |
| `PasswordChangeView` | Allows users to change their password. | `form_valid()`, `form_invalid()` |
| `PasswordResetView` | Handles password reset requests. | `form_valid()`, `get_success_url()` |
| `PasswordResetConfirmView` | Confirms and sets a new password. | `form_valid()`, `form_invalid()` |
| `PasswordResetDoneView` | Displays success message after password reset email is sent. | `get()` |
| `PasswordResetCompleteView` | Final page after resetting password. | `get()` |

---

### **8. Mixins Used with CBVs**  
Mixins extend CBVs by adding reusable behavior.

| Mixin Name | Purpose |
|-----------|---------|
| `LoginRequiredMixin` | Restricts access to authenticated users. |
| `PermissionRequiredMixin` | Ensures users have specific permissions. |
| `FormMixin` | Adds form-handling capabilities. |
| `ContextMixin` | Passes additional context data to templates. |
| `SingleObjectMixin` | Retrieves a single object for CBVs. |
| `MultipleObjectMixin` | Manages multiple objects in CBVs. |
| `SuccessMessageMixin` | Displays success messages after form submission. |
| `UserPassesTestMixin` | Restricts access based on a test function. |
| `AccessMixin` | Base class for authentication mixins. |
| `AjaxResponseMixin` | Handles AJAX responses in views. |


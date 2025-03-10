## **Django Generic Class-Based Views (GCBVs)**  

Django provides **Generic Class-Based Views (GCBVs)** to simplify common patterns like displaying, creating, updating, and deleting objects. These views reduce boilerplate code by automatically handling logic based on the model and form provided.  

---

### **1. Generic Display Views**  
These views retrieve and display model data without modification.  

| View Name | Description | Primary Methods |  
|-----------|------------|-----------------|  
| `DetailView` | Displays a single object based on a model. | `get_object()`, `get_context_data()` |  
| `ListView` | Displays a paginated list of objects from a model. | `get_queryset()`, `paginate_queryset()` |  

---

### **2. Generic Editing Views**  
These views handle creating, updating, and deleting model objects.  

| View Name | Description | Primary Methods |  
|-----------|------------|-----------------|  
| `CreateView` | Handles form submission for object creation. | `form_valid()`, `form_invalid()`, `get_success_url()` |  
| `UpdateView` | Allows editing an existing object using a form. | `get_object()`, `form_valid()`, `get_success_url()` |  
| `DeleteView` | Confirms and deletes an object, then redirects. | `get_object()`, `delete()`, `get_success_url()` |  

---

### **3. Generic Form Handling Views**  
These views manage form submission processes without requiring a model.  

| View Name | Description | Primary Methods |  
|-----------|------------|-----------------|  
| `FormView` | Displays and processes a form without a model. | `get_form_class()`, `form_valid()`, `form_invalid()` |  

---

### **4. Generic Date-Based Views**  
These views retrieve and display objects based on a date field.  

| View Name | Description | Primary Methods |  
|-----------|------------|-----------------|  
| `ArchiveIndexView` | Displays objects sorted by date. | `get_dated_items()` |  
| `YearArchiveView` | Displays objects from a specific year. | `get_year()` |  
| `MonthArchiveView` | Displays objects from a specific month. | `get_month()` |  
| `WeekArchiveView` | Displays objects from a specific week. | `get_week()` |  
| `DayArchiveView` | Displays objects from a specific day. | `get_day()` |  
| `TodayArchiveView` | Displays objects from the current day. | `get_today()` |  
| `DateDetailView` | Displays a single object filtered by date. | `get_object()` |  

---

### **5. Generic Processing Views**  
These views provide additional functionality such as redirection and handling form submissions.  

| View Name | Description | Primary Methods |  
|-----------|------------|-----------------|  
| `RedirectView` | Redirects users to another URL. | `get_redirect_url()`, `get()`, `post()` |  
| `ProcessFormView` | Processes form submissions without rendering a template. | `post()`, `form_valid()` |  

---

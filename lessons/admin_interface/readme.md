## **Admin Interface in Django**

The Django Admin is a built-in web interface for managing application data. It provides CRUD functionality for registered models and is commonly used for internal/admin operations.

---

### **1. Purpose**

* Manage model data via a GUI.
* Automate content management without extra coding.
* Provide authentication and permission controls.

---

### **2. Enabling the Admin**

Ensure `'django.contrib.admin'`, `'django.contrib.auth'`, and `'django.contrib.contenttypes'` are in `INSTALLED_APPS`.

In `urls.py`:

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

---

### **3. Creating a Superuser**

To access the admin interface:

```bash
python manage.py createsuperuser
```

Then login at:
`http://localhost:8000/admin/`

---

### **4. Registering Models**

In `admin.py` of your app:

```python
from django.contrib import admin
from .models import Product

admin.site.register(Product)
```

Now `Product` is manageable through the admin UI.

---

### **5. Customizing Admin Interface**

Use `ModelAdmin` classes for customization.

```python
class ProductAdmin(admin.ModelAdmin):
    list_display = ('name', 'price', 'in_stock')
    search_fields = ['name']
    list_filter = ['in_stock']

admin.site.register(Product, ProductAdmin)
```

---

### **6. Useful `ModelAdmin` Options**

| Option            | Description            |
| ----------------- | ---------------------- |
| `list_display`    | Columns in list view   |
| `list_filter`     | Sidebar filters        |
| `search_fields`   | Search bar fields      |
| `ordering`        | Default sort order     |
| `readonly_fields` | Non-editable fields    |
| `fields`          | Form field layout      |
| `exclude`         | Fields to hide in form |

---

### **7. Fieldsets for Form Layout**

Customize admin form grouping:

```python
fieldsets = (
    ('Basic Info', {
        'fields': ('name', 'price')
    }),
    ('Availability', {
        'fields': ('in_stock',)
    }),
)
```

---

### **8. Inline Model Editing**

Allow related objects to be edited inline.

```python
class ItemInline(admin.TabularInline):  # or StackedInline
    model = Item

class OrderAdmin(admin.ModelAdmin):
    inlines = [ItemInline]
```

---

### **9. Custom Actions**

Add bulk actions to model list:

```python
def mark_out_of_stock(modeladmin, request, queryset):
    queryset.update(in_stock=False)

mark_out_of_stock.short_description = "Mark selected items as out of stock"

class ProductAdmin(admin.ModelAdmin):
    actions = [mark_out_of_stock]
```

---

### **10. Admin Site Branding**

In `urls.py` or `admin.py`:

```python
admin.site.site_header = "Inventory Admin"
admin.site.site_title = "Inventory Management"
admin.site.index_title = "Welcome to Admin Panel"
```

---

### **11. Security Features**

* Admin site is protected by authentication.
* Uses CSRF protection by default.
* Enforces permission checks (e.g., view/add/change/delete).

---

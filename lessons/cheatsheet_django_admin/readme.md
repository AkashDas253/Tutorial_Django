### **Django Admin Cheatsheet**  

Django Admin is a built-in interface for managing database records.  

---

## **1. Enabling Django Admin**  

### **Add `django.contrib.admin` in `INSTALLED_APPS` (`settings.py`)**  
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

### **Run Migrations**  
```sh
python manage.py migrate
```

### **Create Superuser**  
```sh
python manage.py createsuperuser
```
- Enter **username, email, and password**.

### **Start the Server & Access Admin Panel**  
```sh
python manage.py runserver
```
- Open **`http://127.0.0.1:8000/admin/`** in the browser.

---

## **2. Registering Models in Admin**  

### **Basic Model Registration (`admin.py`)**  
```python
from django.contrib import admin
from .models import Book

admin.site.register(Book)
```

### **Customizing Admin Panel (`admin.py`)**  
```python
class BookAdmin(admin.ModelAdmin):
    list_display = ('title', 'author', 'published_date', 'price')
    list_filter = ('author', 'published_date')
    search_fields = ('title', 'author__name')
    ordering = ('-published_date',)

admin.site.register(Book, BookAdmin)
```

| **Option** | **Description** |
|------------|----------------|
| `list_display` | Columns displayed in the list view. |
| `list_filter` | Sidebar filters. |
| `search_fields` | Search bar fields. |
| `ordering` | Default ordering. |

---

## **3. Inline Models** (Editing Related Models in the Same Form)  
```python
class ChapterInline(admin.TabularInline):
    model = Chapter
    extra = 1  # Number of empty forms

class BookAdmin(admin.ModelAdmin):
    inlines = [ChapterInline]

admin.site.register(Book, BookAdmin)
```

---

## **4. Custom Actions**  
```python
class BookAdmin(admin.ModelAdmin):
    actions = ['mark_as_published']

    def mark_as_published(self, request, queryset):
        queryset.update(published_date="2024-01-01")
    mark_as_published.short_description = "Mark selected books as published"

admin.site.register(Book, BookAdmin)
```

---

## **5. Customizing the Admin Dashboard**  

### **Changing Admin Site Title (`admin.py`)**  
```python
admin.site.site_header = "My Admin Dashboard"
admin.site.site_title = "Admin Portal"
admin.site.index_title = "Welcome to Django Admin"
```

---

## **6. Restricting Access**  

### **Limit Admin Access to Specific Users (`admin.py`)**  
```python
class BookAdmin(admin.ModelAdmin):
    def has_add_permission(self, request):
        return request.user.is_superuser

    def has_delete_permission(self, request, obj=None):
        return False  # Disable delete option

admin.site.register(Book, BookAdmin)
```

---

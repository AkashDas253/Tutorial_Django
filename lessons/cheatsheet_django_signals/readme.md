### **Django Signals Cheatsheet**  

Django Signals allow decoupled components to communicate by triggering events when certain actions occur.

---

## **1. Importing Signals**
```python
from django.db.models.signals import pre_save, post_save, pre_delete, post_delete
from django.core.signals import request_started, request_finished
from django.dispatch import receiver
```

| **Signal** | **Triggers When** |
|-----------|----------------|
| `pre_save` | Before saving a model instance. |
| `post_save` | After saving a model instance. |
| `pre_delete` | Before deleting a model instance. |
| `post_delete` | After deleting a model instance. |
| `request_started` | When an HTTP request starts. |
| `request_finished` | When an HTTP request ends. |

---

## **2. Connecting Signals**  

### **Using the `@receiver` Decorator**
```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from myapp.models import UserProfile

@receiver(post_save, sender=UserProfile)
def create_profile(sender, instance, created, **kwargs):
    if created:
        print(f"UserProfile created for {instance}")
```

### **Manually Connecting a Signal**
```python
post_save.connect(create_profile, sender=UserProfile)
```

---

## **3. Disconnecting Signals**  
```python
post_save.disconnect(create_profile, sender=UserProfile)
```

---

## **4. Built-in Django Signals**  

| **Signal** | **Triggered When** |
|-----------|--------------------|
| `pre_migrate` | Before running migrations. |
| `post_migrate` | After running migrations. |
| `request_started` | Before processing an HTTP request. |
| `request_finished` | After processing an HTTP request. |
| `got_request_exception` | When an exception occurs in a request. |

---

## **5. Example: Logging Model Changes**  

### **Log User Creation (`signals.py`)**
```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth.models import User

@receiver(post_save, sender=User)
def log_user_creation(sender, instance, created, **kwargs):
    if created:
        print(f"New user created: {instance.username}")
```

### **Register Signals in `apps.py`**
```python
from django.apps import AppConfig

class MyAppConfig(AppConfig):
    default_auto_field = "django.db.models.BigAutoField"
    name = "myapp"

    def ready(self):
        import myapp.signals  # Import signals
```

---

## **6. Example: Automatically Deleting Related Data**  

### **Delete User Profile When User is Deleted**
```python
from django.db.models.signals import post_delete
from django.dispatch import receiver
from django.contrib.auth.models import User
from myapp.models import UserProfile

@receiver(post_delete, sender=User)
def delete_profile(sender, instance, **kwargs):
    instance.userprofile.delete()
```

---

## **7. Preventing Circular Import Issues**  
- Import signals inside `ready()` in `apps.py`, not in `models.py`.

---

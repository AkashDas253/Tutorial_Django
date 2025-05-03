## **Signals in Django**

Signals allow decoupled applications to get notified when certain actions occur elsewhere in the framework. They enable **event-driven programming** and promote **loose coupling**.

---

### **1. Purpose of Signals**

Used to execute code when certain events happen, such as:

* Model save/delete
* User login/logout
* Request start/finish

---

### **2. Signal Components**

| Component    | Description                                            |
| ------------ | ------------------------------------------------------ |
| **Signal**   | The event source (e.g., `post_save`)                   |
| **Receiver** | A function that handles the signal                     |
| **Sender**   | The object that sends the signal (e.g., a model class) |

---

### **3. Built-in Signals**

| Signal                                                     | Description                 |
| ---------------------------------------------------------- | --------------------------- |
| `pre_save`                                                 | Before a model’s `save()`   |
| `post_save`                                                | After a model’s `save()`    |
| `pre_delete`                                               | Before a model’s `delete()` |
| `post_delete`                                              | After a model’s `delete()`  |
| `m2m_changed`                                              | Many-to-many field changes  |
| `pre_migrate` / `post_migrate`                             | Before/after migrations     |
| `request_started`                                          | When a request starts       |
| `request_finished`                                         | When a request ends         |
| `got_request_exception`                                    | On exception during request |
| `user_logged_in` / `user_logged_out` / `user_login_failed` | Auth events                 |

---

### **4. Connecting a Signal**

#### Method 1: Using a decorator

```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from .models import MyModel

@receiver(post_save, sender=MyModel)
def my_handler(sender, instance, created, **kwargs):
    if created:
        print("New instance created:", instance)
```

#### Method 2: Using `connect()`

```python
def my_handler(sender, instance, **kwargs):
    ...

post_save.connect(my_handler, sender=MyModel)
```

---

### **5. Signal Parameters**

* `sender`: The model class sending the signal
* `instance`: The actual model instance being saved/deleted
* `created`: Boolean (True if new object)
* `kwargs`: Extra data

---

### **6. Best Practice: AppConfig Setup**

In `apps.py` of your app:

```python
def ready(self):
    import myapp.signals
```

Ensures signals are registered when the app is ready.

---

### **7. Disconnecting Signals**

```python
post_save.disconnect(my_handler, sender=MyModel)
```

Used for testing or avoiding duplicate signals.

---

### **8. Custom Signals**

#### Define a signal:

```python
from django.dispatch import Signal

my_signal = Signal()
```

#### Send the signal:

```python
my_signal.send(sender=None, arg1=value1)
```

#### Receive it:

```python
@receiver(my_signal)
def my_custom_handler(sender, **kwargs):
    print("Custom signal received")
```

---

### **9. Use Cases**

* Auto profile creation on user registration
* Logging activities
* Updating caches
* Sending emails/notifications

---

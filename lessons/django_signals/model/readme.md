## Django Model Signals

---

### 🧠 Concept

**Model signals** are a subset of Django's built-in signals that respond to changes in model instances. They allow actions to be triggered automatically when **data-related events** like saving or deleting occur.

Model signals enable **loose coupling** between the model and the logic that runs when changes happen (like sending emails, updating logs, syncing data).

---

### 📦 Available Model Signals

| Signal          | Description                                                            |
|------------------|------------------------------------------------------------------------|
| `pre_save`       | Triggered **before** a model instance is saved                        |
| `post_save`      | Triggered **after** a model instance is saved                         |
| `pre_delete`     | Triggered **before** a model instance is deleted                      |
| `post_delete`    | Triggered **after** a model instance is deleted                       |
| `m2m_changed`    | Triggered when a many-to-many field on a model is changed             |
| `class_prepared` | Triggered when a model class is fully loaded and prepared by Django   |

---

### 🛠️ Usage Syntax

#### Connecting with Decorator

```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from myapp.models import MyModel

@receiver(post_save, sender=MyModel)
def my_model_saved(sender, instance, created, **kwargs):
    if created:
        print("New instance created")
```

#### Connecting with `.connect()`

```python
from django.db.models.signals import pre_delete
from myapp.models import MyModel

def log_delete(sender, instance, **kwargs):
    print(f"Deleting: {instance}")

pre_delete.connect(log_delete, sender=MyModel)
```

---

### 🧾 Signal Arguments (kwargs)

| Argument       | Description                                                    |
|----------------|----------------------------------------------------------------|
| `sender`       | The model class that sent the signal                           |
| `instance`     | The actual model instance being saved or deleted               |
| `created`      | (For `post_save`) Boolean: `True` if new, `False` if updated   |
| `raw`          | `True` if the model is saved via fixture loading (`manage.py loaddata`) |
| `using`        | Database alias (if multiple databases are used)                |
| `update_fields`| List of fields updated in `update()` call (optional)           |
| `signal`       | The signal itself (usually not used directly)                  |

---

### 🔄 Signal Execution Order

| Action | Order of Execution                           |
|--------|-----------------------------------------------|
| Save   | `pre_save` → Save to DB → `post_save`         |
| Delete | `pre_delete` → Delete from DB → `post_delete` |

---

### 🔗 Many-to-Many Signal: `m2m_changed`

Used to track when an object’s many-to-many relationships are modified.

#### Actions

| Action Type  | Description                       |
|--------------|-----------------------------------|
| `pre_add`    | Before items are added            |
| `post_add`   | After items are added             |
| `pre_remove` | Before items are removed          |
| `post_remove`| After items are removed           |
| `pre_clear`  | Before the relationship is cleared|
| `post_clear` | After the relationship is cleared |

#### Syntax

```python
from django.db.models.signals import m2m_changed
from myapp.models import Book

@receiver(m2m_changed, sender=Book.authors.through)
def authors_changed(sender, instance, action, **kwargs):
    if action == "post_add":
        print("Author added")
```

---

### 📌 Best Practices

| Tip                                    | Reason                                                  |
|----------------------------------------|----------------------------------------------------------|
| Use `apps.py` to connect signals       | Ensures signals are registered during app initialization |
| Avoid placing signal connections in `models.py` | Prevents circular imports and loading issues           |
| Use signals only for side-effects      | Keep business logic out of signals                      |
| Be mindful of performance              | Signals execute synchronously—avoid heavy logic         |
| Always check `created` for `post_save` | To distinguish between insert and update                |

---

### 🎯 Use Cases

| Use Case                            | Signal             |
|-------------------------------------|--------------------|
| Send welcome email on registration  | `post_save` (User) |
| Delete related files on object delete | `post_delete`     |
| Auto-create related models          | `post_save`        |
| Sync external systems on data save  | `post_save`        |
| Prevent deletion in specific cases  | `pre_delete`       |

---

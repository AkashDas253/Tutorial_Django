## Django Database/Migration Signals

---

### 🧠 Concept

**Database/Migration signals** in Django are triggered during the **migration process**, allowing developers to run **custom logic before or after applying migrations**. These are particularly useful for:

- Initial data loading
- Logging migration activities
- Ensuring constraints or settings are ready
- Creating default admin users, permissions, or data

---

### 📦 Available Signals

| Signal Name    | Trigger Timing              | Common Use Cases                                     |
|----------------|-----------------------------|------------------------------------------------------|
| `pre_migrate`  | Before running any migration| Set up prerequisites, log start, custom pre-checks   |
| `post_migrate` | After running migrations    | Create default data, permissions, relationships      |

---

### 🧾 Signal Arguments

| Argument       | Description                                                   |
|----------------|---------------------------------------------------------------|
| `sender`       | The app config instance that triggered the migration          |
| `app_config`   | AppConfig object of the app being migrated                    |
| `verbosity`    | Verbosity level (as passed to `migrate` command)              |
| `interactive`  | Boolean indicating if user input is allowed during the process|
| `using`        | The database alias being used for migration                   |
| `plan`         | List of migration operations to be applied                    |

---

### 🛠️ Usage Syntax

#### `post_migrate` example: create default groups

```python
from django.db.models.signals import post_migrate
from django.contrib.auth.models import Group
from django.dispatch import receiver

@receiver(post_migrate)
def create_default_groups(sender, **kwargs):
    Group.objects.get_or_create(name='Editors')
    Group.objects.get_or_create(name='Viewers')
```

#### `pre_migrate` example: log migration intent

```python
from django.db.models.signals import pre_migrate
import logging

logger = logging.getLogger(__name__)

@receiver(pre_migrate)
def log_pre_migration(sender, **kwargs):
    logger.info(f"About to migrate app: {sender.name}")
```

---

### 🏗️ Signal Registration Best Practice

Always register migration signals in **`apps.py`**, inside the `ready()` method of your app config:

```python
# myapp/apps.py

from django.apps import AppConfig

class MyAppConfig(AppConfig):
    name = 'myapp'

    def ready(self):
        import myapp.signals  # Ensure signal handlers are loaded
```

---

### ✅ Common `post_migrate` Use Cases

| Task                            | Why Use `post_migrate`?                          |
|---------------------------------|--------------------------------------------------|
| Create default roles or groups  | Ensures roles exist after tables are created     |
| Populate lookup tables          | Initialize data after schema is applied          |
| Create superuser or test data   | Simplifies development and testing environments  |
| Add permissions manually        | Avoids race conditions with auth app migrations  |

---

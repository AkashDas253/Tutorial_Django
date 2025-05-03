## **Celery Integration**

Celery is an asynchronous task queue/job queue used with Django for handling background tasks such as sending emails, processing data, scheduling jobs, etc.

---

### **1. Installation**

Install Celery and message broker (e.g., Redis):

```bash
pip install celery redis
```

---

### **2. Project Structure**

```
myproject/
├── myproject/
│   ├── __init__.py  ← Initialize Celery here
│   └── celery.py    ← Celery app definition
├── app/
│   └── tasks.py     ← Background tasks
```

---

### **3. `myproject/celery.py`**

```python
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'myproject.settings')

app = Celery('myproject')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
```

---

### **4. `myproject/__init__.py`**

```python
from .celery import app as celery_app

__all__ = ['celery_app']
```

---

### **5. Django Settings for Celery**

In `settings.py`:

```python
CELERY_BROKER_URL = 'redis://localhost:6379/0'          # Redis as broker
CELERY_RESULT_BACKEND = 'redis://localhost:6379/0'       # Store results
CELERY_ACCEPT_CONTENT = ['json']
CELERY_TASK_SERIALIZER = 'json'
```

---

### **6. Creating Tasks**

In `app/tasks.py`:

```python
from celery import shared_task

@shared_task
def add(x, y):
    return x + y
```

---

### **7. Calling Tasks**

From anywhere in Django:

```python
from app.tasks import add
add.delay(4, 6)  # Asynchronous call
```

---

### **8. Running Celery Worker**

```bash
celery -A myproject worker --loglevel=info
```

---

### **9. Periodic Tasks (with Celery Beat)**

Install:

```bash
pip install django-celery-beat
```

Add to `INSTALLED_APPS`:

```python
'django_celery_beat',
```

Run migrations:

```bash
python manage.py migrate
```

In `settings.py`:

```python
CELERY_BEAT_SCHEDULER = 'django_celery_beat.schedulers.DatabaseScheduler'
```

Start the scheduler:

```bash
celery -A myproject beat --loglevel=info
```

Configure periodic tasks via Django admin or manually.

---

### **10. Monitoring Tasks (Optional)**

* Use **Flower** to monitor:

```bash
pip install flower
celery -A myproject flower
```

Visit `http://localhost:5555` for UI.

---

### **Celery & Task Queues in Django Cheatsheet**  

Celery is an asynchronous task queue that allows running background tasks in Django. It is commonly used for sending emails, processing files, and running scheduled jobs.  

---

## **1. Installing Celery**  

```sh
pip install celery
```

### **Install Redis (as Message Broker)**
```sh
sudo apt install redis
```
Start Redis:
```sh
redis-server
```

---

## **2. Configuring Celery in Django**  

### **Add Celery to `settings.py`**
```python
CELERY_BROKER_URL = "redis://localhost:6379/0"
CELERY_ACCEPT_CONTENT = ["json"]
CELERY_TASK_SERIALIZER = "json"
```

| **Setting** | **Description** |
|------------|----------------|
| `CELERY_BROKER_URL` | Defines the message broker (Redis, RabbitMQ). |
| `CELERY_ACCEPT_CONTENT` | Formats allowed for messages. |
| `CELERY_TASK_SERIALIZER` | Specifies serialization type (`json`). |

---

## **3. Creating a Celery Instance**  

### **`celery.py` in the Django Project (`project_root/project/celery.py`)**
```python
import os
from celery import Celery

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "project.settings")

celery_app = Celery("project")
celery_app.config_from_object("django.conf:settings", namespace="CELERY")
celery_app.autodiscover_tasks()
```

### **Modify `__init__.py` (`project_root/project/__init__.py`)**
```python
from .celery import celery_app

__all__ = ("celery_app",)
```

---

## **4. Writing Celery Tasks**  

### **Define a Task in `tasks.py` (`app_name/tasks.py`)**
```python
from celery import shared_task
import time

@shared_task
def add(x, y):
    time.sleep(5)  # Simulating delay
    return x + y
```

| **Function** | **Description** |
|-------------|----------------|
| `@shared_task` | Registers a function as a Celery task. |
| `time.sleep()` | Simulates processing delay. |

---

## **5. Running Celery Workers**  

### **Start the Celery Worker**
```sh
celery -A project worker --loglevel=info
```

| **Flag** | **Description** |
|---------|----------------|
| `-A project` | Specifies the Django project. |
| `worker` | Starts the worker process. |
| `--loglevel=info` | Sets logging level. |

---

## **6. Calling Tasks**  

### **Call Tasks in Views or Shell**
```python
from app_name.tasks import add

result = add.delay(10, 20)
print(result.id)  # Task ID
```

### **Check Task Result**
```python
from celery.result import AsyncResult

task = AsyncResult("task_id")
print(task.status)  # PENDING, SUCCESS, FAILURE
print(task.result)  # Task output
```

| **Method** | **Description** |
|-----------|----------------|
| `add.delay(args)` | Calls task asynchronously. |
| `task.status` | Gets task status. |
| `task.result` | Retrieves result (if available). |

---

## **7. Scheduling Periodic Tasks**  

### **Install `celery-beat`**
```sh
pip install django-celery-beat
```

### **Add to `INSTALLED_APPS` (`settings.py`)**
```python
INSTALLED_APPS = [
    "django_celery_beat",
]
```

### **Migrate and Create Beat Scheduler**
```sh
python manage.py migrate django_celery_beat
python manage.py createsuperuser  # Create admin user
```

### **Start Celery Beat**
```sh
celery -A project beat --loglevel=info
```

---

## **8. Configuring Retry & Expiry**  

### **Retry Task on Failure**
```python
@shared_task(bind=True, max_retries=3)
def process_data(self):
    try:
        # Processing logic
        pass
    except Exception as exc:
        raise self.retry(exc=exc, countdown=10)
```

| **Option** | **Description** |
|-----------|----------------|
| `max_retries` | Limits retry attempts. |
| `countdown` | Delay before retry (in seconds). |

---

## **9. Monitoring Celery Tasks**  

### **Flower - Celery Monitoring Tool**
```sh
pip install flower
celery -A project flower
```
Open **http://localhost:5555** to monitor tasks.

---

## **10. Running Celery with Supervisor (For Production)**  

### **Install Supervisor**
```sh
sudo apt install supervisor
```

### **Create Celery Configuration (`/etc/supervisor/conf.d/celery.conf`)**
```
[program:celery]
command=/path/to/venv/bin/celery -A project worker --loglevel=info
directory=/path/to/project/
autostart=true
autorestart=true
stderr_logfile=/var/log/celery.err.log
stdout_logfile=/var/log/celery.out.log
```

### **Reload and Start Supervisor**
```sh
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start celery
```

---

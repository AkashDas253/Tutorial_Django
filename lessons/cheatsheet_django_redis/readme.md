### **Django & Redis Cheatsheet**  

Redis is an in-memory data store used in Django for caching, session storage, and task queues.

---

## **1. Install Redis & Dependencies**  

### **Install Redis on System**
```sh
sudo apt install redis-server  # Ubuntu
brew install redis             # macOS
choco install redis            # Windows
```

### **Install Python Dependencies**
```sh
pip install django-redis
```

---

## **2. Configure Django to Use Redis**  

### **Edit `settings.py`**
```python
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
```

---

## **3. Using Redis as Cache**  

### **Set & Get Cache Data**
```python
from django.core.cache import cache

cache.set("key", "value", timeout=60)  # Store value for 60 seconds
value = cache.get("key")
```

### **Delete Cached Data**
```python
cache.delete("key")
```

### **Cache View Results**
```python
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)  # Cache for 15 minutes
def my_view(request):
    ...
```

---

## **4. Using Redis for Django Sessions**  

### **Configure in `settings.py`**
```python
SESSION_ENGINE = "django.contrib.sessions.backends.cache"
SESSION_CACHE_ALIAS = "default"
```

---

## **5. Using Redis for Celery Task Queue**  

### **Install Celery**
```sh
pip install celery redis
```

### **Configure `celery.py` in Django Project**
```python
import os
from celery import Celery

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "myproject.settings")
app = Celery("myproject")
app.config_from_object("django.conf:settings", namespace="CELERY")
app.autodiscover_tasks()
```

### **Configure Redis as Broker in `settings.py`**
```python
CELERY_BROKER_URL = "redis://127.0.0.1:6379/0"
```

### **Create a Celery Task (`tasks.py`)**
```python
from celery import shared_task

@shared_task
def add(x, y):
    return x + y
```

### **Run Celery Worker**
```sh
celery -A myproject worker --loglevel=info
```

---

## **6. Using Redis as a Rate Limiter**  

### **Throttle Requests (`views.py`)**
```python
from django.core.cache import cache
from django.http import JsonResponse

def rate_limited_view(request):
    ip = request.META["REMOTE_ADDR"]
    key = f"rate_limit:{ip}"
    
    if cache.get(key):
        return JsonResponse({"error": "Too many requests"}, status=429)
    
    cache.set(key, "1", timeout=60)  # Limit one request per minute
    return JsonResponse({"message": "Success"})
```

---

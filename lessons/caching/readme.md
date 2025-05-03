## **Caching in Django**

Caching improves performance by storing computed data so that future requests are faster. Django supports multiple caching backends and levels of granularity.

---

### **1. Types of Caching in Django**

| Level                 | Use Case                              |
| --------------------- | ------------------------------------- |
| **Site-wide cache**   | Cache entire site pages               |
| **Per-view cache**    | Cache output of individual views      |
| **Template fragment** | Cache parts of templates              |
| **Low-level cache**   | Manually store/retrieve data in cache |

---

### **2. Cache Backends**

Defined in `settings.py`:

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',  # In-memory (dev)
        'LOCATION': 'unique-snowflake'
    }
}
```

| Backend                           | Description                          |
| --------------------------------- | ------------------------------------ |
| `LocMemCache`                     | In-memory, per-process (default)     |
| `FileBasedCache`                  | Stores cache in filesystem           |
| `MemcachedCache` / `PyLibMCCache` | Fast, distributed memory-based       |
| `RedisCache`                      | Supports advanced features, reliable |

---

### **3. Site-Wide Caching**

Add middleware:

```python
MIDDLEWARE = [
    'django.middleware.cache.UpdateCacheMiddleware',
    ...
    'django.middleware.cache.FetchFromCacheMiddleware',
]
```

And in `settings.py`:

```python
CACHE_MIDDLEWARE_SECONDS = 600  # Cache duration in seconds
CACHE_MIDDLEWARE_ALIAS = 'default'
CACHE_MIDDLEWARE_KEY_PREFIX = ''
```

---

### **4. Per-View Caching**

Use decorator `cache_page`:

```python
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)  # 15 minutes
def my_view(request):
    ...
```

---

### **5. Template Fragment Caching**

Cache only a part of a template:

```django
{% load cache %}
{% cache 600 sidebar %}
    <!-- Sidebar HTML here -->
{% endcache %}
```

---

### **6. Low-Level Caching API**

Manually store and retrieve from cache:

```python
from django.core.cache import cache

# Set cache
cache.set('key', 'value', timeout=300)

# Get cache
value = cache.get('key', default='fallback')

# Delete cache
cache.delete('key')
```

---

### **7. Using Redis as Cache Backend**

Install Redis and `django-redis`:

```bash
pip install django-redis
```

In `settings.py`:

```python
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',
        'OPTIONS': {
            'CLIENT_CLASS': 'django_redis.client.DefaultClient',
        }
    }
}
```

---

### **8. Versioning and Key Prefixing**

Use `key_prefix` or version control for cache keys:

```python
cache.set('key', 'value', version=2)
cache.get('key', version=2)
```

---

### **9. Cache Invalidation**

Manually clear specific cache keys when related data changes:

```python
cache.delete('mykey')
```

Or use versioned keys, or signal-based cache clearing for models.

---

### **10. Debugging Cache Usage**

Use Django debug toolbar to see cache hits/misses. Also:

* Log or print cache access
* Monitor Redis or Memcached stats

---

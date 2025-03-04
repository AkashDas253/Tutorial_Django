### **Django Multi-Tenancy Cheatsheet**  

Multi-tenancy allows a single Django application to serve multiple customers (tenants) with data isolation.

---

## **1. Multi-Tenancy Approaches**  

- **Schema-Based**: Each tenant has a separate database schema (PostgreSQL recommended).  
- **Database-Based**: Each tenant has a separate database.  
- **Row-Based**: A single database with tenant ID fields to distinguish data.  

---

## **2. Schema-Based Multi-Tenancy (Using `django-tenants`)**  

### **Install `django-tenants`**  
```sh
pip install django-tenants
```

### **Update `settings.py`**  
```python
INSTALLED_APPS = [
    "django_tenants",
    "myapp",
]

DATABASES = {
    "default": {
        "ENGINE": "django_tenants.postgresql_backend",
        "NAME": "mydb",
        "USER": "user",
        "PASSWORD": "password",
        "HOST": "localhost",
        "PORT": "5432",
    }
}

DATABASE_ROUTERS = ("django_tenants.routers.TenantSyncRouter",)
```

---

### **Create Tenant Model (`models.py`)**  
```python
from django_tenants.models import TenantMixin, DomainMixin
from django.db import models

class Tenant(TenantMixin):
    name = models.CharField(max_length=100)

class Domain(DomainMixin):
    pass
```

---

### **Create Migration & Apply**  
```sh
python manage.py makemigrations
python manage.py migrate_schemas
```

---

### **Create a New Tenant**  
```python
from myapp.models import Tenant, Domain

tenant = Tenant(schema_name="tenant1", name="Tenant One")
tenant.save()

domain = Domain(domain="tenant1.localhost", tenant=tenant)
domain.save()
```

---

## **3. Database-Based Multi-Tenancy (Using `DATABASES` Configuration)**  

### **Define Multiple Databases in `settings.py`**  
```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "main_db",
        "USER": "user",
        "PASSWORD": "password",
        "HOST": "localhost",
        "PORT": "5432",
    },
    "tenant1": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "tenant1_db",
        "USER": "user",
        "PASSWORD": "password",
        "HOST": "localhost",
        "PORT": "5432",
    }
}

DATABASE_ROUTERS = ["myapp.db_router.DatabaseRouter"]
```

### **Create a Database Router (`db_router.py`)**  
```python
class DatabaseRouter:
    def db_for_read(self, model, **hints):
        return hints.get("tenant") or "default"

    def db_for_write(self, model, **hints):
        return hints.get("tenant") or "default"
```

### **Query a Specific Tenant’s Database**  
```python
from django.db import connections

with connections["tenant1"].cursor() as cursor:
    cursor.execute("SELECT * FROM myapp_table;")
```

---

## **4. Row-Based Multi-Tenancy (Using Tenant Filtering)**  

### **Modify Models (`models.py`)**  
```python
from django.db import models

class Tenant(models.Model):
    name = models.CharField(max_length=100)

class Product(models.Model):
    name = models.CharField(max_length=100)
    tenant = models.ForeignKey(Tenant, on_delete=models.CASCADE)
```

### **Filter Data Per Tenant**  
```python
tenant = Tenant.objects.get(name="Tenant1")
products = Product.objects.filter(tenant=tenant)
```

---

## **5. Middleware for Tenant Identification**  

### **Create `middleware.py`**  
```python
from django.utils.deprecation import MiddlewareMixin
from myapp.models import Tenant

class TenantMiddleware(MiddlewareMixin):
    def process_request(self, request):
        domain = request.get_host().split(".")[0]
        request.tenant = Tenant.objects.filter(name=domain).first()
```

### **Update `settings.py`**  
```python
MIDDLEWARE = [
    "myapp.middleware.TenantMiddleware",
]
```

---

## **6. Running Tenant-Specific Migrations**  
```sh
python manage.py migrate_schemas --shared
python manage.py migrate_schemas --tenant
```

---

## **7. Debugging Multi-Tenant Issues**  
```sh
python manage.py shell
```
```python
from myapp.models import Tenant
print(Tenant.objects.all())  # Verify tenants
```

---

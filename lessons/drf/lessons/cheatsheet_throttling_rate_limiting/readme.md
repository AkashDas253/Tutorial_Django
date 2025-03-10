## **Django Rest Framework (DRF) - Throttling & Rate Limiting**  

### **Overview**  
Throttling and rate limiting control the number of API requests a user can make within a specific time frame. This prevents abuse, improves performance, and ensures fair resource usage.

---

### **Throttling in DRF**  
Throttling restricts API access based on predefined rules.  

| Throttle Type | Description |
|--------------|-------------|
| **AnonRateThrottle** | Limits unauthenticated users (`'anon'`). |
| **UserRateThrottle** | Limits authenticated users (`'user'`). |
| **ScopedRateThrottle** | Applies throttling per view or action (`'scope'`). |
| **Custom Throttling** | Defines custom rules based on business logic. |

---

### **Global Throttle Configuration (`settings.py`)**  
```python
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES': [
        'rest_framework.throttling.AnonRateThrottle',
        'rest_framework.throttling.UserRateThrottle',
    ],
    'DEFAULT_THROTTLE_RATES': {
        'anon': '100/day',
        'user': '1000/day'
    }
}
```
- **Anonymous users**: 100 requests per day.  
- **Authenticated users**: 1000 requests per day.  

---

### **Scoped Throttling**  
Scoped throttling applies different limits to specific views.  

**Example: ScopedRateThrottle in a View**  
```python
from rest_framework.throttling import ScopedRateThrottle
from rest_framework.views import APIView
from rest_framework.response import Response

class ProductListView(APIView):
    throttle_classes = [ScopedRateThrottle]
    throttle_scope = 'product_list'

    def get(self, request):
        return Response({"message": "Product list"})

```
**Configuration (`settings.py`)**  
```python
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_RATES': {
        'product_list': '50/hour'
    }
}
```
- Limits `ProductListView` to **50 requests per hour**.  

---

### **Custom Throttling**  
Custom throttling allows defining throttling behavior based on request attributes.  

**Example: Custom Throttling Class**  
```python
from rest_framework.throttling import BaseThrottle

class CustomThrottle(BaseThrottle):
    def allow_request(self, request, view):
        return request.user.is_staff  # Only staff users can access

```
- Grants access **only to staff users**.  

---

### **Rate Limiting vs Throttling**  
| Feature | Throttling | Rate Limiting |
|---------|-----------|--------------|
| Scope | Per API endpoint or user type | Server-wide request control |
| Purpose | Prevents API abuse | Protects overall system load |
| Enforcement | Applied at request level | Managed at infrastructure level (e.g., Nginx, Cloudflare) |

---

### **Best Practices**  
- Set **lower limits** for anonymous users to prevent abuse.  
- Use **ScopedRateThrottle** to apply different limits per API.  
- Implement **Custom Throttling** for special access conditions.  
- Use **external rate limiting tools** (e.g., Nginx, AWS API Gateway) for broader control.  

---

### **Conclusion**  
Throttling and rate limiting in DRF ensure controlled API usage, enhance security, and improve system stability by preventing excessive API requests.
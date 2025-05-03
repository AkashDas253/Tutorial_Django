## Throttling in Django REST Framework

### Purpose

Throttling is a technique used to control the rate at which clients can access an API. It helps protect the API from being overwhelmed by too many requests in a short time, ensuring fair usage among clients and preventing abuse.

Throttling is commonly used for:

* Limiting the number of requests a user can make to an API in a given time frame.
* Preventing excessive load on the server and protecting resources.
* Managing traffic for free-tier or limited access accounts.

---

### Throttling Classes in DRF

Django REST Framework comes with several built-in throttling classes that can be applied to limit the rate of requests based on various conditions.

#### 1. **AnonRateThrottle**

* **Description**: This throttling class limits the rate of requests for **anonymous** users (those who are not authenticated).
* **Default**: Allows `100 requests per 24 hours` and `10 requests per second`.
* **Use Case**: Useful for public APIs where you want to limit requests from non-authenticated users to prevent abuse.

**Example**:

```python
from rest_framework.throttling import AnonRateThrottle
from rest_framework.views import APIView
from rest_framework.response import Response

class ExampleView(APIView):
    throttle_classes = [AnonRateThrottle]

    def get(self, request):
        return Response({"message": "This is rate-limited for anonymous users"})
```

#### 2. **UserRateThrottle**

* **Description**: Limits the rate of requests for **authenticated** users based on their user account.
* **Default**: Allows `100 requests per 24 hours` and `10 requests per second`.
* **Use Case**: Used when you want to throttle individual authenticated users.

**Example**:

```python
from rest_framework.throttling import UserRateThrottle
from rest_framework.views import APIView
from rest_framework.response import Response

class ExampleView(APIView):
    throttle_classes = [UserRateThrottle]

    def get(self, request):
        return Response({"message": "This is rate-limited for authenticated users"})
```

#### 3. **ScopedRateThrottle**

* **Description**: Allows throttling with **different rates** for different scopes, which can be useful for setting limits for specific views or actions.
* **Use Case**: When you want to define rate limits per specific view or action.

**Example**:

```python
from rest_framework.throttling import ScopedRateThrottle
from rest_framework.views import APIView
from rest_framework.response import Response

class ExampleView(APIView):
    throttle_classes = [ScopedRateThrottle]
    throttle_scope = 'example_scope'

    def get(self, request):
        return Response({"message": "Scoped rate-limiting"})
```

---

### Configuring Throttling

Throttling can be configured globally, per view, or per viewset.

#### 1. **Global Throttling**

You can apply throttling globally by modifying the `DEFAULT_THROTTLE_CLASSES` and `DEFAULT_THROTTLE_RATES` settings in the Django settings file (`settings.py`).

**Example**:

```python
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES': [
        'rest_framework.throttling.AnonRateThrottle',
        'rest_framework.throttling.UserRateThrottle',
    ],
    'DEFAULT_THROTTLE_RATES': {
        'anon': '100/day',
        'user': '1000/day',
    }
}
```

#### 2. **Per-View Throttling**

You can set throttling on a per-view basis by using the `throttle_classes` attribute in the view.

**Example**:

```python
from rest_framework.views import APIView
from rest_framework.throttling import AnonRateThrottle

class ExampleView(APIView):
    throttle_classes = [AnonRateThrottle]

    def get(self, request):
        return Response({"message": "This view has throttling applied"})
```

#### 3. **Per-ViewSet Throttling**

For viewsets, you can specify throttling by defining the `throttle_classes` at the viewset class level.

**Example**:

```python
from rest_framework import viewsets
from rest_framework.throttling import UserRateThrottle

class ExampleViewSet(viewsets.ModelViewSet):
    throttle_classes = [UserRateThrottle]

    def get_queryset(self):
        return Example.objects.all()
```

---

### Custom Throttling

You can define custom throttling classes by subclassing the `BaseThrottle` class and implementing the `allow_request` method. The `allow_request` method should return `True` if the request should be allowed or `False` if it should be blocked.

#### **Creating Custom Throttle Class**

**Example**:

```python
from rest_framework.throttling import BaseThrottle
from datetime import datetime, timedelta

class CustomRateThrottle(BaseThrottle):
    def __init__(self):
        self.rate_limit = 5  # 5 requests per minute
        self.time_window = timedelta(minutes=1)
        self.requests = {}

    def allow_request(self, request, view):
        user = request.user

        # If the user has made a request in the last time window, allow it
        if user in self.requests:
            request_time = self.requests[user]
            if datetime.now() - request_time < self.time_window:
                return False  # Deny the request
        # Update the time of the user's last request
        self.requests[user] = datetime.now()
        return True  # Allow the request
```

**Usage in a View**:

```python
from rest_framework.views import APIView

class ExampleView(APIView):
    throttle_classes = [CustomRateThrottle]

    def get(self, request):
        return Response({"message": "Custom throttling applied"})
```

---

### Throttling Rate Formats

When configuring throttling, DRF uses the following formats for rate limits:

* **`<number>/<time period>`**: Example `100/day` means 100 requests per day.
* **`<number>/<time period>` per user**: Example `10/minute` means 10 requests per minute per user.
* **`<number>/<time period>` per IP**: Example `5/second` means 5 requests per second for any IP address.

---

### Summary of Key Concepts

* **Throttle Classes**:

  * **AnonRateThrottle**: Limits requests from anonymous users.
  * **UserRateThrottle**: Limits requests from authenticated users.
  * **ScopedRateThrottle**: Allows throttling with different rates for different views or actions.

* **Configuring Throttling**:

  * **Global Throttling**: Apply throttling settings to the entire project.
  * **Per-View Throttling**: Apply throttling to individual views.
  * **Per-ViewSet Throttling**: Apply throttling to viewsets.

* **Custom Throttling**: You can create custom throttling classes by inheriting from `BaseThrottle` and implementing the `allow_request` method.

* **Rate Formats**: Throttling rate is specified in a format like `<number>/<time period>`, e.g., `100/day` or `5/second`.

---

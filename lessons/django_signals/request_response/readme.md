## Django Request/Response Signals

---

### 🧠 Concept

**Request/Response signals** allow you to hook into Django’s HTTP request/response lifecycle. These signals fire at different points during the handling of an HTTP request—such as when a request starts, finishes, or when an exception occurs.

These are useful for:
- Logging
- Monitoring
- Resource tracking (e.g., DB connections, API usage)
- Performance measurement
- Debugging or custom middleware-like behavior

---

### 📦 Available Request/Response Signals

| Signal Name            | Trigger Point                              | Common Use Cases                             |
|------------------------|---------------------------------------------|----------------------------------------------|
| `request_started`      | When Django receives an HTTP request        | Start timers, log IPs                        |
| `request_finished`     | When Django finishes handling the request   | Stop timers, close resources                 |
| `got_request_exception`| When an exception is raised during a request| Log or report exceptions to a service        |

---

### 🛠️ Usage Syntax

#### `request_started`

```python
from django.core.signals import request_started
from django.dispatch import receiver

@receiver(request_started)
def log_start(sender, **kwargs):
    print("New request started")
```

#### `request_finished`

```python
from django.core.signals import request_finished
from django.dispatch import receiver

@receiver(request_finished)
def log_finish(sender, **kwargs):
    print("Request finished")
```

#### `got_request_exception`

```python
from django.core.signals import got_request_exception
from django.dispatch import receiver
import logging

logger = logging.getLogger(__name__)

@receiver(got_request_exception)
def handle_exception(sender, request, **kwargs):
    logger.error("Exception occurred", exc_info=True)
```

---

### 🧾 Signal Arguments

| Signal                | Key Arguments                      | Description                                  |
|------------------------|------------------------------------|----------------------------------------------|
| `request_started`      | `environ`                          | WSGI environment dict of the request         |
| `request_finished`     | None                               | No arguments                                 |
| `got_request_exception`| `request`                          | The `HttpRequest` instance that caused error |

---

### 🧪 Best Practices

| Tip                                  | Reason                                                 |
|--------------------------------------|--------------------------------------------------------|
| Avoid blocking operations            | Signals are synchronous—slow signals delay the request |
| Use for logging, not business logic  | Keeps request handling clean and fast                  |
| Use `got_request_exception` for alerting | Integrate Sentry, Slack, or logging alerts         |
| Pair `request_started` and `finished`| Great for tracking performance                        |

---

### 🎯 Use Cases

| Use Case                           | Signal               |
|------------------------------------|----------------------|
| Log request lifecycle              | `request_started`, `request_finished` |
| Report errors to monitoring tools  | `got_request_exception` |
| Measure request time               | Start timer on `request_started`, stop on `request_finished` |
| Log IPs or request metadata        | `request_started`    |
| Clean up open DB or file handles   | `request_finished`   |

---

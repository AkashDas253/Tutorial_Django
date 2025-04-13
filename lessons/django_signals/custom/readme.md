## Django Custom Signals

---

### 🧠 Concept

**Custom signals** in Django let you define your own notification system between components of your app. They're useful when:

- You want **loose coupling** between parts of your code
- Some event **isn't already covered** by Django's built-in signals
- You need to **broadcast an event** (like "order placed" or "payment completed") to multiple listeners

---

### 🧾 Creating a Custom Signal

#### Define the signal (typically in `signals.py`)

```python
from django.dispatch import Signal

# Define a custom signal with optional arguments
order_placed = Signal()  # No arguments
order_completed = Signal(providing_args=["order", "user"])  # (older Django style)
```

> ⚠️ Note: `providing_args` is deprecated in Django 4.0+ and no longer required.

---

### 🛠️ Sending the Signal

Use `send()` or `send_robust()` to trigger your custom signal.

```python
from .signals import order_placed

# Inside some view or service logic
order_placed.send(sender=self.__class__)
```

```python
from .signals import order_completed

order_completed.send(
    sender=self.__class__,
    order=order_instance,
    user=request.user
)
```

---

### 📡 Receiving the Signal

Define and register a receiver to listen for the signal:

```python
from django.dispatch import receiver
from .signals import order_completed

@receiver(order_completed)
def send_invoice_email(sender, order, user, **kwargs):
    print(f"Invoice sent to {user.email} for order #{order.id}")
```

---

### ✅ Best Practices

| Tip                                | Why                                                   |
|------------------------------------|--------------------------------------------------------|
| Use `send_robust()` for safety     | Avoid breaking app if one receiver raises an error     |
| Register signals in `apps.py`      | Ensures loading at app startup                        |
| Keep handlers separate             | Improves modularity and testability                   |
| Name signals based on **event**    | e.g., `user_registered`, `item_purchased`             |

---

### 🧪 Testing Custom Signals

```python
from django.test import TestCase
from django.dispatch import Signal
from unittest.mock import MagicMock

my_signal = Signal()

class CustomSignalTest(TestCase):
    def test_custom_signal(self):
        mock_handler = MagicMock()
        my_signal.connect(mock_handler)

        my_signal.send(sender=self.__class__, data='test')
        mock_handler.assert_called_once()
```

---

### 🔄 `send()` vs `send_robust()`

| Method           | Description                            |
|------------------|----------------------------------------|
| `send()`         | Raises errors from any receiver         |
| `send_robust()`  | Catches and logs exceptions, continues  |

---

### 🧩 Use Case Examples

| Event Name         | Description                        | Example Listener                  |
|--------------------|------------------------------------|------------------------------------|
| `user_verified`    | Triggered after email verification | Send welcome email                 |
| `comment_flagged`  | A comment has been flagged         | Notify moderators                  |
| `payment_failed`   | Payment processor returned failure | Alert user or log to dashboard     |

---

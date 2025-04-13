## Django Testing Signals

---

### 🧠 Concept

Testing Django signals ensures your **signal handlers are properly triggered** and behave as expected when a signal fires. Since signals run independently of views or forms, **explicit testing** is needed to validate their behavior.

---

### 🧪 Key Objectives of Signal Testing

- Ensure signal **gets called** on model actions (save, delete, migrate)
- Validate **side-effects** (e.g., logging, object creation)
- Prevent **duplicate signal triggers**
- Maintain **test isolation** using mocking or disconnecting signals

---

### 📦 Tools for Testing Signals

| Tool                  | Use Case                                |
|-----------------------|------------------------------------------|
| `unittest.mock`       | Spy on or temporarily disable signal logic |
| `@override_settings`  | Configure or disable signals in test runs |
| `Signal.disconnect()` | Temporarily disable signal behavior      |
| `Signal.has_listeners()` | Check if listeners are connected     |

---

### ✅ General Signal Test Structure

```python
from django.test import TestCase
from django.db.models.signals import post_save
from unittest.mock import patch
from myapp.models import MyModel

class SignalTestCase(TestCase):

    @patch('myapp.signals.my_model_saved')
    def test_post_save_signal_called(self, mock_handler):
        obj = MyModel.objects.create(name='Test')
        self.assertTrue(mock_handler.called)
```

---

### 🔐 Disconnecting Signals Temporarily

```python
from django.db.models.signals import post_save
from myapp.models import MyModel
from myapp.signals import my_model_saved

class SignalTestCase(TestCase):
    def test_without_signal(self):
        post_save.disconnect(receiver=my_model_saved, sender=MyModel)
        MyModel.objects.create(name='No Signal')
        post_save.connect(receiver=my_model_saved, sender=MyModel)
```

---

### 🧾 Mocking Best Practices

| Practice                         | Reason                                                  |
|----------------------------------|----------------------------------------------------------|
| Patch at **import path**, not function | Ensures correct function is intercepted               |
| Use `assert_called_once_with()`  | Confirm correct arguments were passed                   |
| Wrap handlers in reusable functions | Makes them easier to patch and test                    |

---

### 🧪 Testing Exception Signals

```python
from django.core.signals import got_request_exception
from django.test import RequestFactory, TestCase
from unittest.mock import patch

class ExceptionSignalTest(TestCase):
    def test_exception_signal(self):
        factory = RequestFactory()
        request = factory.get('/nonexistent-url/')

        with patch('myapp.signals.handle_exception') as mock_handler:
            self.client.get('/nonexistent-url/')
            self.assertTrue(mock_handler.called)
```

---

### 🧼 Cleanup & Isolation

| Tip                              | Benefit                        |
|----------------------------------|--------------------------------|
| Disconnect signals in `setUp()`  | Prevents test crosstalk        |
| Use `@patch` context managers    | Auto-clean after test runs     |
| Avoid `print()` in handlers      | Keep test output clean         |

---

### 🧰 Common Assertions

| Method                     | Purpose                                 |
|----------------------------|------------------------------------------|
| `mock.called`              | Check if signal handler was triggered    |
| `mock.call_count`          | Ensure signal fired expected # of times  |
| `mock.assert_called_once()`| Confirm it was triggered exactly once    |
| `mock.assert_called_with()`| Confirm correct parameters were passed   |

---

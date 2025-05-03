## **Testing in Django**

Django provides a rich framework for writing unit tests, integration tests, and functional tests using Python’s built-in `unittest` module and Django-specific tools.

---

### **1. Why Test in Django?**

* Validate that code works as expected.
* Automate regression testing.
* Ensure views, models, and forms behave correctly.
* Improve maintainability and reliability.

---

### **2. Test Case Structure**

Django tests are subclasses of `django.test.TestCase`:

```python
from django.test import TestCase
from .models import Book

class BookModelTest(TestCase):
    def setUp(self):
        Book.objects.create(title="Django Basics")

    def test_book_title(self):
        book = Book.objects.get(title="Django Basics")
        self.assertEqual(book.title, "Django Basics")
```

---

### **3. Types of Tests**

| Type                  | Description                                                                    |
| --------------------- | ------------------------------------------------------------------------------ |
| **Unit Tests**        | Test individual components like functions or methods.                          |
| **Integration Tests** | Test multiple parts working together.                                          |
| **Functional Tests**  | Simulate user interactions with the full app (often with tools like Selenium). |

---

### **4. Running Tests**

Use the `manage.py` command:

```bash
python manage.py test
```

To test specific app:

```bash
python manage.py test myapp
```

---

### **5. Assertions**

| Method                                        | Checks                      |
| --------------------------------------------- | --------------------------- |
| `assertEqual(a, b)`                           | `a == b`                    |
| `assertTrue(x)`                               | `bool(x) is True`           |
| `assertFalse(x)`                              | `bool(x) is False`          |
| `assertContains(response, text)`              | Response includes `text`    |
| `assertRedirects(response, url)`              | Response redirects to `url` |
| `assertTemplateUsed(response, template_name)` | Template rendered           |

---

### **6. Django Test Client**

Used to simulate GET/POST requests:

```python
from django.test import Client

client = Client()
response = client.get('/books/')
self.assertEqual(response.status_code, 200)
```

You can test form submissions, session variables, login/logout, etc.

---

### **7. Fixtures**

Preload test data from JSON/YAML/CSV:

```bash
python manage.py dumpdata myapp.Model > my_fixture.json
```

In the test:

```python
class MyTest(TestCase):
    fixtures = ['my_fixture.json']
```

---

### **8. Database Isolation**

* Each test is run in a transaction.
* Database is reset before each test class.
* `setUp()` is called before every test.
* `tearDown()` cleans up afterward.

---

### **9. Testing Views and Templates**

```python
def test_home_view(self):
    response = self.client.get('/')
    self.assertEqual(response.status_code, 200)
    self.assertTemplateUsed(response, 'home.html')
```

---

### **10. Testing Forms**

```python
def test_form_valid(self):
    form = MyForm(data={'name': 'John'})
    self.assertTrue(form.is_valid())
```

---

### **11. Testing Models**

```python
def test_str_method(self):
    book = Book.objects.create(title='Sample')
    self.assertEqual(str(book), 'Sample')
```

---

### **12. Coverage Tool**

Install and run coverage:

```bash
pip install coverage
coverage run manage.py test
coverage report
coverage html  # for HTML report
```

---

### **13. Mocking and Patching**

Use `unittest.mock` for mocking:

```python
from unittest.mock import patch

@patch('myapp.utils.send_email')
def test_email_called(self, mock_send):
    my_function()
    mock_send.assert_called_once()
```

---

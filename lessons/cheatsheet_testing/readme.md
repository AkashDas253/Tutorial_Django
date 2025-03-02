### **Testing in Django Cheatsheet**  

Testing in Django ensures code reliability and prevents regressions. Django provides a built-in testing framework based on Python’s `unittest`.

---

## **1. Setting Up Tests**  

### **Create a `tests.py` File in Each App**  
- By default, Django looks for `tests.py` in each app.

### **Use `TestCase` for Database Testing**  
```python
from django.test import TestCase
from myapp.models import Book

class BookModelTest(TestCase):
    def setUp(self):
        Book.objects.create(title="Django Testing", author="John Doe")

    def test_book_creation(self):
        book = Book.objects.get(title="Django Testing")
        self.assertEqual(book.author, "John Doe")
```

| **Feature**  | **Description**  |
|-------------|----------------|
| `setUp()`   | Runs before each test.  |
| `TestCase`  | Allows database rollback after tests.  |
| `assertEqual(a, b)` | Checks if `a == b`. |

---

## **2. Running Tests**  

### **Run All Tests**
```sh
python manage.py test
```

### **Run Tests for a Specific App**
```sh
python manage.py test myapp
```

### **Run a Specific Test Case**
```sh
python manage.py test myapp.tests.BookModelTest
```

### **Run a Single Test Method**
```sh
python manage.py test myapp.tests.BookModelTest.test_book_creation
```

---

## **3. Testing Models**  

### **Common Assertions**
| **Assertion** | **Description** |
|--------------|----------------|
| `assertEqual(a, b)` | Check if `a == b` |
| `assertNotEqual(a, b)` | Check if `a != b` |
| `assertTrue(x)` | Check if `x is True` |
| `assertFalse(x)` | Check if `x is False` |
| `assertIsNone(x)` | Check if `x is None` |
| `assertIsNotNone(x)` | Check if `x is not None` |

---

## **4. Testing Views**  

### **Using Django’s `Client`**
```python
from django.test import TestCase, Client

class BookViewTest(TestCase):
    def setUp(self):
        self.client = Client()

    def test_homepage(self):
        response = self.client.get("/")
        self.assertEqual(response.status_code, 200)
```

| **Method** | **Description** |
|-----------|----------------|
| `self.client.get(url)` | Simulates a GET request. |
| `self.client.post(url, data)` | Simulates a POST request. |
| `response.status_code` | Checks response code (e.g., 200, 404). |
| `response.context` | Accesses context data in views. |

---

## **5. Testing Forms**  

```python
from django.test import TestCase
from myapp.forms import BookForm

class BookFormTest(TestCase):
    def test_valid_form(self):
        form = BookForm(data={"title": "Django", "author": "John"})
        self.assertTrue(form.is_valid())

    def test_invalid_form(self):
        form = BookForm(data={"title": ""})  # Missing required field
        self.assertFalse(form.is_valid())
```

| **Feature** | **Description** |
|------------|----------------|
| `form.is_valid()` | Returns `True` if valid. |
| `form.errors` | Contains validation errors. |

---

## **6. Testing Authentication**  

### **Create User in `setUp()`**
```python
from django.contrib.auth.models import User

class AuthTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(username="testuser", password="password123")

    def test_login(self):
        login = self.client.login(username="testuser", password="password123")
        self.assertTrue(login)
```

| **Feature** | **Description** |
|------------|----------------|
| `create_user()` | Creates a test user. |
| `client.login()` | Simulates user login. |

---

## **7. Testing Django REST API**  

### **Install `djangorestframework`**  
```sh
pip install djangorestframework
```

### **Use `APITestCase`**
```python
from rest_framework.test import APITestCase
from django.contrib.auth.models import User

class APIAuthTest(APITestCase):
    def setUp(self):
        self.user = User.objects.create_user(username="apiuser", password="pass")

    def test_token_auth(self):
        response = self.client.post("/api/token/", {"username": "apiuser", "password": "pass"})
        self.assertEqual(response.status_code, 200)
```

| **Feature** | **Description** |
|------------|----------------|
| `APITestCase` | Used for DRF API testing. |
| `self.client.post(url, data)` | Simulates API request. |

---

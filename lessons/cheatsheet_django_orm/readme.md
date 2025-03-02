### **Django ORM Cheatsheet**  

Django's **Object-Relational Mapper (ORM)** allows interaction with databases using Python instead of SQL.  

---

## **1. Model Definition (`models.py`)**  

```python
from django.db import models

class Author(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField(unique=True)

class Book(models.Model):
    title = models.CharField(max_length=200)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)
    published_date = models.DateField()
    price = models.DecimalField(max_digits=6, decimal_places=2)
```

---

## **2. Database Operations**  

### **Creating Records**  
```python
author = Author.objects.create(name="John Doe", email="john@example.com")
book = Book(title="Django ORM Guide", author=author, published_date="2024-01-01", price=19.99)
book.save()
```

### **Reading Records (Querying)**  
```python
all_books = Book.objects.all()  # Get all records
book = Book.objects.get(id=1)  # Get a single record
books_by_author = Book.objects.filter(author__name="John Doe")  # Filtering
latest_books = Book.objects.order_by('-published_date')[:5]  # Ordering
exists = Book.objects.filter(title="Django ORM Guide").exists()  # Check existence
```

### **Updating Records**  
```python
book = Book.objects.get(id=1)
book.price = 25.99
book.save()
```
```python
Book.objects.filter(author__name="John Doe").update(price=29.99)
```

### **Deleting Records**  
```python
book = Book.objects.get(id=1)
book.delete()
```
```python
Book.objects.filter(author__name="John Doe").delete()
```

---

## **3. QuerySet Methods**  

| Method | Description |
|--------|------------|
| `.all()` | Returns all records. |
| `.filter(**conditions)` | Returns filtered records. |
| `.exclude(**conditions)` | Excludes records matching conditions. |
| `.order_by(*fields)` | Orders results. |
| `.values(*fields)` | Returns QuerySet as dictionaries. |
| `.distinct()` | Removes duplicate records. |
| `.count()` | Counts matching records. |
| `.first() / .last()` | Returns the first/last record. |
| `.exists()` | Checks if QuerySet has records. |

---

## **4. Relationships in ORM**  

### **ForeignKey (One-to-Many)**  
```python
class Book(models.Model):
    author = models.ForeignKey(Author, on_delete=models.CASCADE)
```
```python
author = Author.objects.get(id=1)
books = author.book_set.all()  # Reverse lookup
```

### **ManyToManyField (Many-to-Many)**  
```python
class Category(models.Model):
    name = models.CharField(max_length=50)

class Book(models.Model):
    categories = models.ManyToManyField(Category)
```
```python
book.categories.add(category)
book.categories.remove(category)
```

### **OneToOneField (One-to-One)**  
```python
class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
```
```python
profile = Profile.objects.get(user=user)
```

---

## **5. Aggregation & Annotation**  
```python
from django.db.models import Count, Sum, Avg, Max, Min

total_books = Book.objects.count()
max_price = Book.objects.aggregate(Max('price'))
average_price = Book.objects.aggregate(Avg('price'))
author_book_count = Author.objects.annotate(book_count=Count('book'))
```

---

## **6. Raw SQL Queries**  
```python
from django.db import connection

with connection.cursor() as cursor:
    cursor.execute("SELECT * FROM my_app_book WHERE price > %s", [20])
    books = cursor.fetchall()
```

---

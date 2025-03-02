### **Django Management Commands Cheatsheet**  

Django provides a way to create custom management commands that can be executed using `manage.py`. These commands help automate tasks like data imports, scheduled jobs, and debugging.

---

## **1. Creating a Custom Management Command**  

### **File Structure**  
```
myapp/
│── management/
│   ├── __init__.py
│   ├── commands/
│   │   ├── __init__.py
│   │   ├── mycommand.py
```

| **Folder/File** | **Purpose** |
|---------------|------------|
| `management/` | Required for management commands. |
| `commands/` | Stores custom command scripts. |
| `mycommand.py` | The actual management command file. |

---

## **2. Writing a Custom Command**  

### **Basic Example (`mycommand.py`)**
```python
from django.core.management.base import BaseCommand

class Command(BaseCommand):
    help = "Displays a custom message"

    def handle(self, *args, **kwargs):
        self.stdout.write("Hello from custom command!")
```

| **Method** | **Purpose** |
|-----------|------------|
| `handle()` | The main method executed when running the command. |
| `self.stdout.write()` | Prints output to the console. |

### **Running the Command**
```sh
python manage.py mycommand
```

---

## **3. Adding Arguments**  

### **Example with Arguments**
```python
from django.core.management.base import BaseCommand

class Command(BaseCommand):
    help = "Prints a custom message"

    def add_arguments(self, parser):
        parser.add_argument("name", type=str, help="Name to display")
        parser.add_argument("--greeting", type=str, default="Hello", help="Custom greeting")

    def handle(self, *args, **options):
        name = options["name"]
        greeting = options["greeting"]
        self.stdout.write(f"{greeting}, {name}!")
```

### **Usage**
```sh
python manage.py mycommand John --greeting "Hi"
```
**Output:**  
```
Hi, John!
```

| **Argument Type** | **Example Usage** |
|------------------|------------------|
| Positional Argument | `python manage.py mycommand John` |
| Optional Argument | `python manage.py mycommand John --greeting "Hi"` |

---

## **4. Handling Database Operations**  

```python
from django.core.management.base import BaseCommand
from myapp.models import Book

class Command(BaseCommand):
    help = "Lists all books"

    def handle(self, *args, **kwargs):
        books = Book.objects.all()
        for book in books:
            self.stdout.write(f"{book.id}: {book.title}")
```

| **Use Case** | **Example** |
|-------------|------------|
| Query database | `Book.objects.all()` |
| Modify data | `Book.objects.create(title="Django")` |

---

## **5. Running Commands Periodically**  

### **Using `cron` or `celery`**
- Add an entry in **crontab**:
```sh
0 0 * * * /path/to/venv/bin/python /path/to/project/manage.py mycommand
```
- Or use **Celery beat** for scheduling.

---

## **6. Logging and Errors**  

```python
import logging
from django.core.management.base import BaseCommand

logger = logging.getLogger(__name__)

class Command(BaseCommand):
    help = "Example command with logging"

    def handle(self, *args, **kwargs):
        try:
            # Simulating an error
            raise ValueError("Something went wrong!")
        except Exception as e:
            logger.error(f"Error: {e}")
            self.stderr.write(self.style.ERROR(f"Error: {e}"))
```

| **Feature** | **Description** |
|------------|----------------|
| `logger.error()` | Logs errors to Django’s log system. |
| `self.style.ERROR()` | Displays an error message in red. |

---

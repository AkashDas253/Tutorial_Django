## **Custom Management Commands**

Custom management commands allow developers to create their own CLI commands for recurring tasks, automation, or batch processing using Django's `manage.py`.

---

### **1. Folder Structure**

Inside any installed Django app:

```
your_app/
├── management/
│   └── commands/
│       └── your_command.py
```

> Must follow: `management/commands/your_command.py`
> `__init__.py` files are required in `management/` and `commands/`.

---

### **2. Example Command: `say_hello`**

**File**: `your_app/management/commands/say_hello.py`

```python
from django.core.management.base import BaseCommand

class Command(BaseCommand):
    help = 'Prints Hello message'

    def handle(self, *args, **kwargs):
        self.stdout.write("Hello from custom command!")
```

Run using:

```bash
python manage.py say_hello
```

---

### **3. Adding Arguments**

```python
from django.core.management.base import BaseCommand

class Command(BaseCommand):
    help = 'Greet a person by name'

    def add_arguments(self, parser):
        parser.add_argument('name', type=str)

    def handle(self, *args, **options):
        name = options['name']
        self.stdout.write(f"Hello, {name}!")
```

Run with:

```bash
python manage.py say_hello John
```

---

### **4. Optional and Named Arguments**

```python
def add_arguments(self, parser):
    parser.add_argument('--repeat', type=int, default=1)

def handle(self, *args, **options):
    for _ in range(options['repeat']):
        self.stdout.write("Hello!")
```

Run with:

```bash
python manage.py say_hello --repeat 3
```

---

### **5. Handling Errors**

```python
from django.core.management.base import CommandError

if some_error_condition:
    raise CommandError('Something went wrong')
```

---

### **6. Using ORM in Commands**

Custom commands can use all Django ORM operations:

```python
from myapp.models import Book

def handle(self, *args, **options):
    books = Book.objects.all()
    for book in books:
        self.stdout.write(book.title)
```

---

### **7. Output Types**

* `self.stdout.write()` — Normal output
* `self.stderr.write()` — Error output
* Use color with `style`:

```python
self.stdout.write(self.style.SUCCESS('Success message'))
self.stdout.write(self.style.ERROR('Error message'))
```

---

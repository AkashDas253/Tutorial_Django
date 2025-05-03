## **Migrations in Django**

Migrations are Django's way of propagating changes made to models into the database schema. They keep database tables in sync with the current state of models.

---

### **1. Purpose**

* Reflect model changes (create/update/delete) into the database.
* Track changes in version-controlled migration files.
* Allow reversible and incremental schema changes.

---

### **2. Creating Migrations**

Command:

```bash
python manage.py makemigrations
```

* Detects changes in `models.py`
* Creates migration files in `app_name/migrations/`

Example output:

```
Migrations for 'store':
  store/migrations/0001_initial.py
    - Create model Product
```

---

### **3. Applying Migrations**

Command:

```bash
python manage.py migrate
```

* Applies all unapplied migrations to the database.
* Runs in order based on dependencies.

---

### **4. Migration Files**

Each migration file contains a subclass of `Migration` and operations.

Example:

```python
from django.db import migrations, models

class Migration(migrations.Migration):
    initial = True

    operations = [
        migrations.CreateModel(
            name='Product',
            fields=[
                ('id', models.AutoField(auto_created=True, primary_key=True)),
                ('name', models.CharField(max_length=100)),
                ('price', models.DecimalField(max_digits=10, decimal_places=2)),
            ],
        ),
    ]
```

---

### **5. Common Operations**

| Operation     | Description              |
| ------------- | ------------------------ |
| `CreateModel` | Create a new model/table |
| `AddField`    | Add a column             |
| `RemoveField` | Delete a column          |
| `RenameField` | Rename a column          |
| `AlterField`  | Modify field attributes  |
| `DeleteModel` | Drop a model/table       |

---

### **6. Show Migrations**

View migration status:

```bash
python manage.py showmigrations
```

Example output:

```
store
 [X] 0001_initial
 [ ] 0002_add_discount_field
```

---

### **7. Applying Specific Migrations**

To a specific app:

```bash
python manage.py migrate store
```

To a specific migration:

```bash
python manage.py migrate store 0001
```

---

### **8. Rolling Back Migrations**

Undo last migration:

```bash
python manage.py migrate store 0001
```

Reset entire database (use with caution):

```bash
python manage.py migrate zero
```

---

### **9. Fake Migrations**

Mark as applied without running:

```bash
python manage.py migrate --fake store 0001
```

Useful for syncing manually altered databases.

---

### **10. Squashing Migrations**

Combine multiple migrations into one:

```bash
python manage.py squashmigrations store 0001 0005
```

Improves performance and reduces file clutter.

---

### **11. Dependencies and Order**

Each migration has a `dependencies` attribute that ensures they are applied in the correct order.

Example:

```python
dependencies = [
    ('store', '0001_initial'),
]
```

---

### **Django Migrations Cheatsheet**  

#### **What Are Migrations?**  
- Track changes in database schema using version control.  
- Auto-generated from models using Django's ORM.  

#### **Migration Commands**  

| Command | Description |
|---------|------------|
| `python manage.py makemigrations` | Creates migration files from model changes. |
| `python manage.py migrate` | Applies migrations to the database. |
| `python manage.py showmigrations` | Lists available migrations. |
| `python manage.py sqlmigrate app_name migration_number` | Shows raw SQL for a migration. |
| `python manage.py makemigrations --dry-run` | Shows migrations without creating files. |
| `python manage.py migrate --fake app_name` | Marks migrations as applied without executing. |
| `python manage.py migrate app_name migration_number` | Rolls back to a specific migration. |

#### **Migration File Structure (`0001_initial.py`)**  
- Stored in `migrations/` folder inside each app.  
- Uses `operations` like `CreateModel`, `AlterField`, etc.  

```python
from django.db import migrations, models

class Migration(migrations.Migration):

    dependencies = []

    operations = [
        migrations.CreateModel(
            name='MyModel',
            fields=[
                ('id', models.AutoField(primary_key=True)),
                ('name', models.CharField(max_length=100)),
            ],
        ),
    ]
```

#### **Rolling Back Migrations**  
| Action | Command |
|--------|---------|
| Undo last migration | `python manage.py migrate app_name previous_migration` |
| Reset all migrations | `python manage.py migrate app_name zero` |
| Fake rollback | `python manage.py migrate app_name previous_migration --fake` |

#### **Making & Applying Migrations for a Specific App**  
```sh
python manage.py makemigrations my_app
python manage.py migrate my_app
```

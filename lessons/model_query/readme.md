## Django Model Query Commands

### Basic Retrieval
- `Model.objects.all()`: Retrieve all objects.
- `Model.objects.get(**kwargs)`: Retrieve a single object that matches the given lookup parameters. Raises `DoesNotExist` if no match is found and `MultipleObjectsReturned` if more than one match is found.

### Filtering and Exclusion
- `Model.objects.filter(**kwargs)`: Retrieve objects that match the given lookup parameters.
- `Model.objects.exclude(**kwargs)`: Retrieve objects that do not match the given lookup parameters.

### Ordering and Limiting
- `Model.objects.order_by(*fields)`: Order the results by the given fields. Use `-` prefix for descending order.
- `Model.objects.first()`: Return the first object in the queryset.
- `Model.objects.last()`: Return the last object in the queryset.

### Aggregation and Annotation
- `Model.objects.aggregate(*args, **kwargs)`: Perform aggregation on the queryset.
- `Model.objects.annotate(*args, **kwargs)`: Annotate each object in the queryset with the provided expressions.

### Counting and Existence
- `Model.objects.count()`: Return the number of objects in the queryset.
- `Model.objects.exists()`: Return `True` if the queryset contains any results, `False` otherwise.

### Distinct and Values
- `Model.objects.distinct()`: Return distinct results.
- `Model.objects.values(*fields)`: Return dictionaries instead of model instances, with only the specified fields.
- `Model.objects.values_list(*fields, flat=False)`: Return tuples instead of model instances, with only the specified fields.

### Bulk Operations
- `Model.objects.bulk_create(objs, batch_size=None, ignore_conflicts=False)`: Insert multiple objects into the database in a single query.
- `Model.objects.bulk_update(objs, fields, batch_size=None)`: Update multiple objects in the database in a single query.

### Update and Delete
- `Model.objects.update(**kwargs)`: Update all objects in the queryset with the given fields.
- `Model.objects.delete()`: Delete all objects in the queryset.

### Related Objects
- `Model.objects.select_related(*fields)`: Perform a SQL join and include the fields of the related objects.
- `Model.objects.prefetch_related(*fields)`: Perform a separate lookup for each relationship and do the joining in Python.

### Raw SQL
- `Model.objects.raw(raw_query, params=None, translations=None)`: Perform a raw SQL query.

### Parameters
- `**kwargs`: Field lookups and their values.
  - `field_name__exact`: Exact match.
  - `field_name__iexact`: Case-insensitive exact match.
  - `field_name__contains`: Contains substring.
  - `field_name__icontains`: Case-insensitive contains substring.
  - `field_name__gt`: Greater than.
  - `field_name__gte`: Greater than or equal to.
  - `field_name__lt`: Less than.
  - `field_name__lte`: Less than or equal to.
  - `field_name__in`: In a given list.
  - `field_name__startswith`: Starts with substring.
  - `field_name__istartswith`: Case-insensitive starts with substring.
  - `field_name__endswith`: Ends with substring.
  - `field_name__iendswith`: Case-insensitive ends with substring.
  - `field_name__range`: Within a given range.
  - `field_name__date`: Date match.
  - `field_name__year`: Year match.
  - `field_name__month`: Month match.
  - `field_name__day`: Day match.
  - `field_name__week_day`: Weekday match.
  - `field_name__isnull`: Is NULL.

- `*fields`: Field names to order by, prefixed with `-` for descending order.
- `*args`: Aggregation functions.

### Complex Lookups (`Q` objects, `F` expressions)
- `Q`: Use `Q` objects to perform complex queries with `AND`, `OR`, and `NOT` operations.
  ```python
  from django.db.models import Q
  Model.objects.filter(Q(field1=value1) | Q(field2=value2))
  ```
- `F`: Use `F` expressions to refer to model field values directly in queries.
  ```python
  from django.db.models import F
  Model.objects.filter(field1=F('field2'))
  ```

### Raw SQL Queries
- `Model.objects.raw(raw_query, params=None, translations=None)`: Perform a raw SQL query.
  ```python
  Model.objects.raw('SELECT * FROM myapp_model WHERE field = %s', [value])
  ```

### Annotations
- `Model.objects.annotate(*args, **kwargs)`: Use `annotate` to add calculated fields to querysets.
  ```python
  from django.db.models import Count
  Model.objects.annotate(num_related=Count('related_model'))
  ```

### Subqueries
- Use subqueries to perform nested queries.
  ```python
  from django.db.models import OuterRef, Subquery
  subquery = Model.objects.filter(related_field=OuterRef('pk')).values('field')[:1]
  Model.objects.annotate(subquery_field=Subquery(subquery))
  ```

### Joins
- Use `select_related` and `prefetch_related` for efficient joins.
  ```python
  Model.objects.select_related('related_model')
  Model.objects.prefetch_related('related_model_set')
  ```

### Aggregations
- Use `aggregate` to perform calculations on querysets.
  ```python
  from django.db.models import Avg
  Model.objects.aggregate(average_field=Avg('field'))
  ```

### Transactions
- Manage database transactions with `atomic`.
  ```python
  from django.db import transaction
  with transaction.atomic():
      # Perform database operations
  ```

### Caching
- Use caching to optimize query performance.
  ```python
  from django.core.cache import cache
  result = cache.get('my_key')
  if not result:
      result = Model.objects.all()
      cache.set('my_key', result)
  ```

### Pagination
- Implement pagination with `Paginator`.
  ```python
  from django.core.paginator import Paginator
  paginator = Paginator(Model.objects.all(), 10)  # Show 10 objects per page
  page = paginator.get_page(1)
  ```

### Custom Managers
- Create custom model managers for advanced query logic.
  ```python
  class CustomManager(models.Manager):
      def custom_query(self):
          return self.filter(field=value)
  ```

### Signals
- Use signals to trigger actions on model events.
  ```python
  from django.db.models.signals import post_save
  from django.dispatch import receiver

  @receiver(post_save, sender=Model)
  def my_handler(sender, instance, created, **kwargs):
      if created:
          # Perform action
  ```

### Migrations
- Manage database schema changes with migrations.
  ```bash
  python manage.py makemigrations
  python manage.py migrate
  ```

### Indexes
- Add indexes to improve query performance.
  ```python
  class Model(models.Model):
      field = models.CharField(max_length=100, db_index=True)
  ```

### Constraints
- Define constraints to enforce data integrity.
  ```python
  class Model(models.Model):
      field = models.IntegerField()
      class Meta:
          constraints = [
              models.CheckConstraint(check=models.Q(field__gte=0), name='field_gte_0')
          ]
  ```

### Permissions
- Implement permissions and access control.
  ```python
  class Model(models.Model):
      class Meta:
          permissions = [
              ("can_do_something", "Can do something")
          ]
  ```
```
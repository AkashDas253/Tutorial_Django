## Django Signals

### 🧠 Concept and Philosophy

**Django signals** are a messaging system that allows **decoupled components** to communicate with each other. When one component changes the state of the application, it can notify other components without needing to directly call them. This pattern is based on the **Observer Pattern**.

- **Decoupling logic**: The code that sends the signal does not know who will receive it.
- **Event-driven**: Actions are triggered in response to events.

---

### 🏛️ Architecture and How It Works

Django signals use the **`Signal`** class from `django.dispatch`. There are three main parts involved:

| Part            | Description                                                                 |
|-----------------|-----------------------------------------------------------------------------|
| **Signal**      | An instance of `Signal` that defines what kind of event to listen to.       |
| **Sender**      | The model or component that emits (sends) the signal.                        |
| **Receiver**    | A function or method that is triggered when the signal is sent.              |

**Flow**:
1. A signal is **defined**.
2. One or more **receivers** are **connected** to the signal.
3. When an event occurs, the **sender emits** the signal.
4. All **connected receivers** are invoked with relevant arguments.

---

### 📦 Built-in Django Signals

| Signal Name          | Triggered When                                                         |
|----------------------|------------------------------------------------------------------------|
| `pre_save`           | Before a model instance is saved to the DB                            |
| `post_save`          | After a model instance is saved to the DB                             |
| `pre_delete`         | Before a model instance is deleted from the DB                        |
| `post_delete`        | After a model instance is deleted from the DB                         |
| `m2m_changed`        | When a many-to-many relationship is modified                          |
| `class_prepared`     | When a model class is prepared (loaded)                               |
| `request_started`    | When a new HTTP request is received                                   |
| `request_finished`   | When a response is sent and request ends                              |
| `got_request_exception` | When an exception occurs during a request                         |
| `pre_migrate`        | Before a migration is applied                                          |
| `post_migrate`       | After a migration is applied                                           |

---

### 🛠️ Syntax and Parameters

#### Connecting a Receiver to a Signal

```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from myapp.models import MyModel

@receiver(post_save, sender=MyModel)
def my_handler(sender, instance, created, **kwargs):
    if created:
        print(f"New instance created: {instance}")
    else:
        print(f"Updated instance: {instance}")
```

#### Parameters

| Parameter   | Description                                           |
|-------------|-------------------------------------------------------|
| `sender`    | The model or component that sends the signal         |
| `instance`  | The actual instance being saved or deleted           |
| `created`   | Boolean indicating if it was created (`True`) or updated (`False`) |
| `**kwargs`  | Additional data such as `signal`, `using`, `update_fields`, etc. |

---

### 🔗 Manual Signal Creation and Usage

#### Define a Custom Signal

```python
from django.dispatch import Signal

my_signal = Signal()  # Or Signal(providing_args=["arg1", "arg2"]) in old versions
```

#### Connect a Receiver

```python
def notify_admin(sender, **kwargs):
    print("Admin notified.")

my_signal.connect(notify_admin)
```

#### Send the Signal

```python
my_signal.send(sender=None)
```

---

### 📎 Signal Connection Methods

| Method         | Description                                                       |
|----------------|-------------------------------------------------------------------|
| `@receiver`    | Decorator to register the function as a signal receiver           |
| `.connect()`   | Programmatically connect a receiver to a signal                   |
| `.disconnect()`| Remove a receiver from a signal                                   |

---

### ⚠️ Caveats and Best Practices

| Practice                     | Reason                                                                 |
|-----------------------------|------------------------------------------------------------------------|
| Use only when needed        | Overuse leads to tangled, hard-to-debug logic                          |
| Avoid business logic in receivers | Keep signals for side-effects, not core workflows                    |
| Connect in `apps.py`        | Ensures signal receivers are connected at app startup                  |
| Avoid circular imports      | Keep signal registration in separate `signals.py`                      |

---

### 🧩 Use Cases

- Sending a welcome email after user registration
- Logging user activity automatically
- Creating related objects on model save
- Updating cache or analytics asynchronously

---
---

## Types of Django Signals

Django provides **two main types** of signals:

---

### 1. **Built-in Signals**

Provided by Django for core framework events like model save, delete, HTTP request lifecycle, etc.

#### 📦 Model Signals

| Signal Name     | Trigger Event                                |
|------------------|----------------------------------------------|
| `pre_save`       | Before a model instance is saved             |
| `post_save`      | After a model instance is saved              |
| `pre_delete`     | Before a model instance is deleted           |
| `post_delete`    | After a model instance is deleted            |
| `m2m_changed`    | When a many-to-many relationship is modified |
| `class_prepared` | When a model class is prepared               |

#### 🌐 Request/Response Signals

| Signal Name             | Trigger Event                                       |
|--------------------------|-----------------------------------------------------|
| `request_started`        | At the beginning of an HTTP request                |
| `request_finished`       | At the end of an HTTP request                      |
| `got_request_exception`  | When an exception is raised during a request       |

#### 🧱 Database/Migration Signals

| Signal Name     | Trigger Event                                |
|------------------|----------------------------------------------|
| `pre_migrate`    | Before running a migration                   |
| `post_migrate`   | After running a migration                    |

#### 🧪 Testing Signals

| Signal Name     | Trigger Event                                |
|------------------|----------------------------------------------|
| `setting_changed`| When Django settings are modified in tests  |
| `template_rendered`| When a template is rendered (used in tests) |

---

### 2. **Custom Signals**

Defined by developers for app-specific events that aren't covered by built-in signals.

#### 🛠️ Example

```python
from django.dispatch import Signal

# Define
payment_successful = Signal()

# Connect
def notify_team(sender, **kwargs):
    print("Payment completed")

payment_successful.connect(notify_team)

# Send
payment_successful.send(sender=None)
```

Use custom signals to:
- Trigger actions after user uploads a file
- Send alerts when a threshold is exceeded
- Trigger chain reactions in loosely coupled apps

---

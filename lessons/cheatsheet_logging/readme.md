### **Logging in Django Cheatsheet**  

Django's logging system is based on Python’s built-in `logging` module. It allows capturing, formatting, and storing logs for debugging and monitoring.

---

## **1. Enabling Logging in Django**  

### **Configure Logging in `settings.py`**
```python
LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,
    "formatters": {
        "verbose": {
            "format": "{levelname} {asctime} {module} {message}",
            "style": "{",
        },
        "simple": {
            "format": "{levelname} {message}",
            "style": "{",
        },
    },
    "handlers": {
        "file": {
            "level": "DEBUG",
            "class": "logging.FileHandler",
            "filename": "debug.log",
            "formatter": "verbose",
        },
        "console": {
            "level": "INFO",
            "class": "logging.StreamHandler",
            "formatter": "simple",
        },
    },
    "loggers": {
        "django": {
            "handlers": ["file", "console"],
            "level": "DEBUG",
            "propagate": True,
        },
    },
}
```

| **Key** | **Description** |
|---------|---------------|
| `formatters` | Defines log message format. |
| `handlers` | Determines where logs are stored (file, console, etc.). |
| `loggers` | Defines log sources and handlers. |

---

## **2. Logging Levels**  

| **Level** | **Value** | **Description** |
|-----------|----------|----------------|
| `DEBUG` | 10 | Detailed information for debugging. |
| `INFO` | 20 | General system messages. |
| `WARNING` | 30 | Something unexpected happened. |
| `ERROR` | 40 | A serious error occurred. |
| `CRITICAL` | 50 | A critical failure requiring immediate attention. |

---

## **3. Logging in Views & Models**  

### **Import and Use Logger**  
```python
import logging

logger = logging.getLogger(__name__)

def my_view(request):
    logger.debug("Debug message")
    logger.info("Info message")
    logger.warning("Warning message")
    logger.error("Error message")
    logger.critical("Critical message")
```

| **Method** | **Description** |
|-----------|----------------|
| `logger.debug(msg)` | Logs a debug message. |
| `logger.info(msg)` | Logs an info message. |
| `logger.warning(msg)` | Logs a warning. |
| `logger.error(msg)` | Logs an error. |
| `logger.critical(msg)` | Logs a critical error. |

---

## **4. Logging to a Custom File**  

### **Define a Custom Logger (`settings.py`)**
```python
LOGGING["handlers"]["custom_file"] = {
    "level": "WARNING",
    "class": "logging.FileHandler",
    "filename": "custom.log",
    "formatter": "verbose",
}

LOGGING["loggers"]["custom_logger"] = {
    "handlers": ["custom_file"],
    "level": "WARNING",
    "propagate": False,
}
```

### **Use the Custom Logger**
```python
custom_logger = logging.getLogger("custom_logger")
custom_logger.warning("This is a warning in custom.log")
```

| **Feature** | **Description** |
|------------|----------------|
| `filename` | Defines the log file name. |
| `propagate` | Prevents log duplication. |

---

## **5. Logging Exceptions**  

### **Use `exception()` to Log Errors**
```python
try:
    1 / 0  # Causes ZeroDivisionError
except Exception as e:
    logger.exception("An error occurred")
```

| **Method** | **Description** |
|-----------|----------------|
| `logger.exception(msg)` | Logs the exception traceback. |

---

## **6. Rotating Log Files**  

### **Use `RotatingFileHandler` (`settings.py`)**
```python
from logging.handlers import RotatingFileHandler

LOGGING["handlers"]["rotating_file"] = {
    "level": "DEBUG",
    "class": "logging.handlers.RotatingFileHandler",
    "filename": "rotating.log",
    "maxBytes": 1024 * 1024 * 5,  # 5MB
    "backupCount": 3,
    "formatter": "verbose",
}

LOGGING["loggers"]["django"] = {
    "handlers": ["rotating_file"],
    "level": "DEBUG",
    "propagate": True,
}
```

| **Option** | **Description** |
|-----------|----------------|
| `maxBytes` | Maximum log file size before rotation. |
| `backupCount` | Number of old log files to keep. |

---

## **7. Sending Logs to Email**  

### **Use `AdminEmailHandler` (`settings.py`)**
```python
LOGGING["handlers"]["mail_admins"] = {
    "level": "ERROR",
    "class": "django.utils.log.AdminEmailHandler",
}

LOGGING["loggers"]["django.request"] = {
    "handlers": ["mail_admins"],
    "level": "ERROR",
    "propagate": False,
}
```

| **Feature** | **Description** |
|------------|----------------|
| `AdminEmailHandler` | Sends logs to `ADMINS` in `settings.py`. |

---

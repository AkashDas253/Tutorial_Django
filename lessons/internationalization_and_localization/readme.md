## **Internationalization (i18n) and Localization (l10n)**

Django provides a robust framework to support translating interfaces (i18n) and formatting data like dates/numbers/currencies according to the user's locale (l10n).

---

### **1. Key Terms**

| Term                            | Meaning                                          |
| ------------------------------- | ------------------------------------------------ |
| **Internationalization (i18n)** | Designing the app to support multiple languages. |
| **Localization (l10n)**         | Adapting the app for a specific language/region. |
| **Translation**                 | Process of converting text to other languages.   |

---

### **2. Settings for i18n/l10n**

In `settings.py`:

```python
USE_I18N = True                # Enables translation machinery
USE_L10N = True                # Enables formatting of dates/numbers
USE_TZ = True                  # Enables timezone support

LANGUAGE_CODE = 'en-us'       # Default language
TIME_ZONE = 'UTC'             # Default timezone

LANGUAGES = [
    ('en', 'English'),
    ('es', 'Spanish'),
    ('fr', 'French'),
]

LOCALE_PATHS = [BASE_DIR / 'locale']  # Path for .po translation files
```

---

### **3. Marking Text for Translation**

| Tag                  | Usage                              |
| -------------------- | ---------------------------------- |
| `gettext()` or `_()` | In Python code                     |
| `gettext_lazy()`     | For lazily evaluated text          |
| `{% trans "text" %}` | In templates                       |
| `{% blocktrans %}`   | For template blocks with variables |

**Examples:**

```python
from django.utils.translation import gettext as _
welcome = _("Welcome")
```

```html
{% load i18n %}
<p>{% trans "Hello, user!" %}</p>
```

---

### **4. Generating Message Files**

Steps:

```bash
django-admin makemessages -l es
```

* Creates `.po` file in `locale/es/LC_MESSAGES/django.po`.

Translate strings manually in `.po` files.

---

### **5. Compiling Messages**

After translation:

```bash
django-admin compilemessages
```

Creates `.mo` files required by Django at runtime.

---

### **6. Switching Languages**

#### Manually (in views):

```python
from django.utils import translation

def my_view(request):
    user_language = 'es'
    translation.activate(user_language)
    response = HttpResponse(...)
    response.set_cookie(settings.LANGUAGE_COOKIE_NAME, user_language)
    return response
```

#### Automatically (via middleware):

Add to `MIDDLEWARE`:

```python
'django.middleware.locale.LocaleMiddleware',
```

Make sure `LocaleMiddleware` comes after `SessionMiddleware` and `CommonMiddleware`.

---

### **7. Localized Formatting**

Django formats dates and numbers based on locale if `USE_L10N = True`.

Template filters:

```django
{{ value|date:"SHORT_DATE_FORMAT" }}
{{ value|floatformat:2 }}
```

---

### **8. URL Internationalization (Optional)**

Enable `i18n_patterns` in `urls.py`:

```python
from django.conf.urls.i18n import i18n_patterns

urlpatterns += i18n_patterns(
    path('admin/', admin.site.urls),
    path('app/', include('myapp.urls')),
)
```

This adds language prefixes like `/en/`, `/es/`.

---

### **9. Translation in Admin**

Django admin uses built-in translations. Ensure `LANGUAGES` includes the desired locales and the browser/user selects the language.

---

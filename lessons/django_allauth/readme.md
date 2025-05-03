## **Django Allauth**

**Django Allauth** is an integrated authentication app for Django that supports:

* Local accounts (username/email + password)
* Social authentication (OAuth1/OAuth2)
* Email verification and account management

---

### **1. Installation**

```bash
pip install django-allauth
```

In `settings.py`, add to `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...
    "django.contrib.sites",
    "allauth",
    "allauth.account",
    "allauth.socialaccount",
    # Add providers as needed
    "allauth.socialaccount.providers.google",
    ...
]
```

Set `SITE_ID = 1` (required by `django.contrib.sites`).

---

### **2. Authentication Backends**

```python
AUTHENTICATION_BACKENDS = [
    "django.contrib.auth.backends.ModelBackend",
    "allauth.account.auth_backends.AuthenticationBackend",
]
```

---

### **3. URLs Configuration**

In `urls.py`:

```python
from django.urls import path, include

urlpatterns = [
    ...
    path("accounts/", include("allauth.urls")),
]
```

---

### **4. Configuration Settings**

| Setting                         | Description                                    |
| ------------------------------- | ---------------------------------------------- |
| `ACCOUNT_AUTHENTICATION_METHOD` | `"username"`, `"email"`, or `"username_email"` |
| `ACCOUNT_EMAIL_REQUIRED`        | Whether to require email                       |
| `ACCOUNT_EMAIL_VERIFICATION`    | `"none"`, `"optional"`, or `"mandatory"`       |
| `ACCOUNT_USERNAME_REQUIRED`     | Whether username is required                   |
| `LOGIN_REDIRECT_URL`            | URL to redirect to after login                 |
| `ACCOUNT_SIGNUP_REDIRECT_URL`   | After signup                                   |
| `ACCOUNT_LOGOUT_REDIRECT_URL`   | After logout                                   |

---

### **5. Social Authentication**

Each provider (Google, GitHub, etc.) needs:

* OAuth client ID/secret
* Redirect/callback URI (`/accounts/{provider}/login/callback/`)
* Admin setup via `SocialApp` model (in Django admin)

You must:

* Register the app on the provider’s developer platform
* Add a `SocialApp` in Django Admin
* Associate it with your Site (from `django.contrib.sites`)

---

### **6. Templates**

Templates can be overridden by placing files in `templates/account/` and `templates/socialaccount/`.

Common templates to override:

* `login.html`
* `signup.html`
* `email.html`
* `password_reset.html`
* `socialaccount/signup.html`

Use `{% load socialaccount %}` and `{% providers_media_js %}` for buttons.

---

### **7. Signals**

| Signal            | Trigger                  |
| ----------------- | ------------------------ |
| `user_signed_up`  | After successful signup  |
| `user_logged_in`  | After login              |
| `user_logged_out` | After logout             |
| `email_confirmed` | After email confirmation |

Example:

```python
from allauth.account.signals import user_signed_up
from django.dispatch import receiver

@receiver(user_signed_up)
def after_signup(request, user, **kwargs):
    # Custom logic
    pass
```

---

### **8. Admin Panel**

* You can manage social applications from `SocialApp`
* Link applications to sites
* Edit login/signup behavior from `ACCOUNT_*` settings

---

### **9. Custom Adapters**

Override logic using custom adapters:

```python
ACCOUNT_ADAPTER = "myapp.adapters.MyAccountAdapter"
SOCIALACCOUNT_ADAPTER = "myapp.adapters.MySocialAccountAdapter"
```

---

### **10. Pros and Use Cases**

| Use Case                       | Allauth Feature                            |
| ------------------------------ | ------------------------------------------ |
| Email-based login              | `ACCOUNT_AUTHENTICATION_METHOD = "email"`  |
| Social auth (Google, GitHub)   | `socialaccount.providers.*`                |
| Email verification             | `ACCOUNT_EMAIL_VERIFICATION = "mandatory"` |
| Password reset, change, logout | Built-in routes and views                  |
| Custom redirect logic          | Custom adapters and signals                |

---

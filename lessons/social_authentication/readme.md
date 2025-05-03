## **Social Authentication**

**Social Authentication** allows users to log in using third-party services like Google, Facebook, GitHub, etc., instead of creating a new account. This is typically implemented using the `django-allauth` or `python-social-auth` packages.

---

### **1. Common Packages**

| Package              | Description                                                                      |
| -------------------- | -------------------------------------------------------------------------------- |
| `django-allauth`     | Most popular for handling social and email/password authentication               |
| `python-social-auth` | Powerful but lower-level than `allauth`; often used via `social-auth-app-django` |

---

### **2. Installation**

For `django-allauth`:

```bash
pip install django-allauth
```

Add to `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'allauth.socialaccount.providers.google',  # Example
]
```

In `settings.py`:

```python
SITE_ID = 1

AUTHENTICATION_BACKENDS = (
    'django.contrib.auth.backends.ModelBackend',
    'allauth.account.auth_backends.AuthenticationBackend',
)
```

---

### **3. URLs Configuration**

In `urls.py`:

```python
from django.urls import path, include

urlpatterns = [
    ...
    path('accounts/', include('allauth.urls')),
]
```

---

### **4. Provider Configuration**

Each provider (Google, Facebook, etc.) requires:

* OAuth credentials (Client ID and Secret)
* Callback URL configuration

Example for **Google**:

* Go to [Google Developers Console](https://console.developers.google.com/)
* Create credentials (OAuth2)
* Set redirect URI: `http://localhost:8000/accounts/google/login/callback/`

Add in Django Admin under `Social Applications`:

* Provider: Google
* Name, Client ID, Secret
* Choose the site (e.g., example.com or localhost)

---

### **5. Usage**

After setup:

* Visit `/accounts/login/` to see available login options.
* Users can sign in via social accounts or traditional email/password.
* All user data is stored in Django’s `User` model and related models in `allauth`.

---

### **6. Customization Options**

| Setting                      | Purpose                                       |
| ---------------------------- | --------------------------------------------- |
| `ACCOUNT_EMAIL_VERIFICATION` | Require email verification                    |
| `SOCIALACCOUNT_AUTO_SIGNUP`  | Auto-create user accounts                     |
| `SOCIALACCOUNT_QUERY_EMAIL`  | Ask for email if provider doesn’t return one  |
| `ACCOUNT_ADAPTER`            | Override logic for user creation, login, etc. |
| `SOCIALACCOUNT_ADAPTER`      | Customize social account behaviors            |

---

### **7. Template Integration**

To customize the login/signup UI:

* Override templates:
  e.g., `templates/account/login.html`, `socialaccount/login.html`

* Use `{% providers_media_js %}` to render provider login buttons

---

### **8. Signals**

You can hook into signals like:

```python
from allauth.account.signals import user_signed_up
from django.dispatch import receiver

@receiver(user_signed_up)
def after_signup(request, user, **kwargs):
    # Add user to group, log signup, etc.
    pass
```

---

### **9. Security & Best Practices**

* Use HTTPS in production for OAuth callbacks.
* Enable scopes carefully (email, profile only).
* Validate email addresses before login if needed.

---

### **10. Common Providers**

| Provider | App Name                                          |
| -------- | ------------------------------------------------- |
| Google   | `allauth.socialaccount.providers.google`          |
| Facebook | `allauth.socialaccount.providers.facebook`        |
| GitHub   | `allauth.socialaccount.providers.github`          |
| Twitter  | `allauth.socialaccount.providers.twitter`         |
| LinkedIn | `allauth.socialaccount.providers.linkedin_oauth2` |

---

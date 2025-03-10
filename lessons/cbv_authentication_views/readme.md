## **Authentication-Based Views in Django**  

### **Overview**  
Django provides built-in authentication views for handling user authentication, including login, logout, password management, and account confirmation. These views are pre-configured and can be customized as needed.  

---

### **Authentication Views and Their Purpose**  

| View | Purpose |
|------|---------|
| `LoginView` | Handles user login. |
| `LogoutView` | Logs out a user. |
| `PasswordChangeView` | Allows users to change passwords. |
| `PasswordChangeDoneView` | Confirms password change completion. |
| `PasswordResetView` | Handles password reset requests. |
| `PasswordResetDoneView` | Informs users about password reset email sent. |
| `PasswordResetConfirmView` | Allows users to reset passwords using a token. |
| `PasswordResetCompleteView` | Confirms password reset completion. |

---

### **LoginView (User Login)**  
Handles user login and authentication.

```python
from django.contrib.auth.views import LoginView

class CustomLoginView(LoginView):
    template_name = "auth/login.html"
```

| Attribute | Purpose |
|-----------|---------|
| `template_name` | Specifies the template for rendering. |
| `authentication_form` | Uses a custom authentication form if provided. |
| `redirect_authenticated_user` | Redirects already logged-in users. |

---

### **LogoutView (User Logout)**  
Handles user logout.

```python
from django.contrib.auth.views import LogoutView

class CustomLogoutView(LogoutView):
    next_page = "/"
```

| Attribute | Purpose |
|-----------|---------|
| `next_page` | Redirects users after logout. |

---

### **Password Change Views**  
Used for authenticated users to change their passwords.

```python
from django.contrib.auth.views import PasswordChangeView, PasswordChangeDoneView

class CustomPasswordChangeView(PasswordChangeView):
    template_name = "auth/password_change.html"
    success_url = "/password-change-done/"

class CustomPasswordChangeDoneView(PasswordChangeDoneView):
    template_name = "auth/password_change_done.html"
```

| Attribute | Purpose |
|-----------|---------|
| `template_name` | Specifies the template for the password change form. |
| `success_url` | Redirects users upon successful password change. |

---

### **Password Reset Views**  
Used when users forget their passwords.

```python
from django.contrib.auth.views import PasswordResetView, PasswordResetDoneView, PasswordResetConfirmView, PasswordResetCompleteView

class CustomPasswordResetView(PasswordResetView):
    template_name = "auth/password_reset.html"
    email_template_name = "auth/password_reset_email.html"
    success_url = "/password-reset-done/"

class CustomPasswordResetDoneView(PasswordResetDoneView):
    template_name = "auth/password_reset_done.html"

class CustomPasswordResetConfirmView(PasswordResetConfirmView):
    template_name = "auth/password_reset_confirm.html"
    success_url = "/password-reset-complete/"

class CustomPasswordResetCompleteView(PasswordResetCompleteView):
    template_name = "auth/password_reset_complete.html"
```

| Attribute | Purpose |
|-----------|---------|
| `email_template_name` | Defines the email template for password reset. |
| `success_url` | Specifies where users are redirected after completing the action. |

---

### **Best Practices for Authentication Views**  

| Best Practice | Reason |
|--------------|--------|
| Use `LoginView` and `LogoutView` for authentication | Reduces the need for custom login/logout logic. |
| Customize password reset views | Provides a branded experience for users. |
| Use `success_url` for redirections | Ensures a smooth user experience after authentication actions. |

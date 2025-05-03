## Authentication in Django REST Framework

### Purpose

Authentication in Django REST Framework (DRF) determines the identity of a user making the request. It ensures that the user has valid credentials and is authorized to perform certain actions on the API. DRF supports several authentication schemes to verify users.

---

### Types of Authentication in DRF

#### 1. **Basic Authentication**

* **Mechanism**: Uses HTTP basic authentication, which sends the username and password encoded in base64 in the `Authorization` header.
* **Implementation**: This is one of the simplest authentication methods where the client sends a base64-encoded string of the format `username:password`.

**Configuration**:

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.BasicAuthentication',
    ],
}
```

#### 2. **Session Authentication**

* **Mechanism**: Uses Django's default session framework to authenticate users based on the session cookie set after login.
* **Use Case**: Often used with web browsers, where sessions are maintained after user login.
* **Authentication Flow**: Upon successful login, Django sets a session ID, and the user is authenticated for subsequent requests.

**Configuration**:

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
    ],
}
```

#### 3. **Token Authentication**

* **Mechanism**: Uses a token to authenticate users. After the user logs in (via a login endpoint), they are given a token. The token is then sent in the `Authorization` header for subsequent requests.
* **Use Case**: Ideal for stateless APIs where user sessions are not stored on the server. Commonly used for mobile apps or SPA (Single Page Applications).

**Configuration**:

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',
    ],
}
```

To use token authentication, you also need to install and migrate the `rest_framework.authtoken` app.

```bash
pip install djangorestframework
pip install djangorestframework-authtoken
python manage.py migrate
```

#### 4. **JWT Authentication (JSON Web Token)**

* **Mechanism**: JWT authentication involves generating a JWT token after the user logs in, which is then sent with each request in the `Authorization` header. The token contains encoded user data, which can be decoded without requiring a session on the server.
* **Use Case**: Used for stateless authentication across multiple services or microservices.

**Implementation**: Typically achieved by using a third-party package like `djangorestframework-simplejwt`.

```bash
pip install djangorestframework-simplejwt
```

**Configuration**:

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
}
```

**Generating and verifying JWT tokens**:

```python
from rest_framework_simplejwt.tokens import RefreshToken

def get_tokens_for_user(user):
    refresh = RefreshToken.for_user(user)
    return {
        'refresh': str(refresh),
        'access': str(refresh.access_token),
    }
```

#### 5. **OAuth2 Authentication**

* **Mechanism**: OAuth2 is an authorization framework that allows users to share access to their resources without exposing credentials. It is often used for third-party integrations (e.g., Google, Facebook).
* **Implementation**: DRF supports OAuth2 through third-party packages like `django-oauth-toolkit`.

**Configuration**:

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'oauth2_provider.ext.rest_framework.OAuth2Authentication',
    ],
}
```

OAuth2 involves issuing access tokens and handling token refresh cycles. It is more complex than other schemes but provides granular permission control.

---

### Custom Authentication Classes

You can create custom authentication schemes by extending DRF’s `BaseAuthentication` class. This is useful for integrating with non-standard or proprietary authentication mechanisms.

#### Example: Custom Authentication

```python
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed

class CustomAuthentication(BaseAuthentication):
    def authenticate(self, request):
        user_token = request.headers.get('X-Custom-Auth')
        if not user_token:
            raise AuthenticationFailed('No token provided')

        # Custom authentication logic
        user = authenticate_with_custom_service(user_token)
        if user is None:
            raise AuthenticationFailed('Invalid token')

        return (user, None)  # (user, auth)
```

**Configuration**:

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'path.to.CustomAuthentication',
    ],
}
```

---

### Authorization and Permissions

Authentication verifies the user’s identity, while **authorization** controls access to specific views or actions based on the authenticated user’s permissions. DRF provides a flexible system for handling authorization.

#### 1. **AllowAny Permission**

* Grants access to any user, regardless of authentication.

```python
from rest_framework.permissions import AllowAny

class ExampleView(APIView):
    permission_classes = [AllowAny]

    def get(self, request):
        return Response({"message": "Anyone can access this"})
```

#### 2. **IsAuthenticated Permission**

* Only authenticated users can access the resource.

```python
from rest_framework.permissions import IsAuthenticated

class ExampleView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        return Response({"message": "Authenticated users only"})
```

#### 3. **IsAdminUser Permission**

* Only users with admin status can access the resource.

```python
from rest_framework.permissions import IsAdminUser

class ExampleView(APIView):
    permission_classes = [IsAdminUser]

    def get(self, request):
        return Response({"message": "Only admin users can access this"})
```

#### 4. **Custom Permissions**

You can also define custom permissions based on user roles or other criteria by extending `BasePermission`.

```python
from rest_framework.permissions import BasePermission

class IsOwner(BasePermission):
    def has_object_permission(self, request, view, obj):
        return obj.owner == request.user
```

**Usage**:

```python
from rest_framework.views import APIView
from rest_framework.permissions import IsAuthenticated

class ExampleView(APIView):
    permission_classes = [IsAuthenticated, IsOwner]

    def get(self, request, pk):
        # Retrieve the object and check if the user is the owner
        obj = get_object_or_404(Example, pk=pk)
        return Response({"message": "You are the owner", "data": obj})
```

---

### Summary of Key Concepts

* **Authentication Schemes**:

  * **Basic Authentication**: Uses username and password in the `Authorization` header.
  * **Session Authentication**: Uses Django sessions, typically for web applications.
  * **Token Authentication**: Stateless authentication using tokens.
  * **JWT Authentication**: JSON Web Tokens for stateless and decentralized authentication.
  * **OAuth2 Authentication**: For third-party integrations and OAuth flows.
* **Custom Authentication**: You can create a custom authentication mechanism by extending `BaseAuthentication`.
* **Permissions**: Define access control through `AllowAny`, `IsAuthenticated`, `IsAdminUser`, or custom permissions like `IsOwner`.

---

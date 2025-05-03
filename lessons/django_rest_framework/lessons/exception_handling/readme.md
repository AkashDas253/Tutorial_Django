## Exception Handling

**Exception Handling** in Django REST Framework (DRF) refers to the mechanism of capturing and managing errors or exceptional conditions that occur during the processing of HTTP requests. DRF provides built-in mechanisms to handle exceptions gracefully, ensuring the response is informative and aligned with HTTP standards.

---

### Concept of Exception Handling

The primary goal of exception handling in DRF is to manage errors in a way that:

* The client receives meaningful error messages.
* The system remains secure and robust, preventing unexpected crashes or leaks of sensitive information.

DRF uses the standard Python exception mechanism but adds additional functionality to deal with common web-based exceptions and provide consistent error responses.

---

### Types of Exceptions in DRF

1. **HTTP Exceptions**: These are exceptions that directly correspond to HTTP errors.

   * Common HTTP exceptions include `NotFound`, `BadRequest`, `PermissionDenied`, etc.
   * DRF provides standard exceptions for common HTTP status codes.

2. **Validation Errors**: Raised when input validation fails, such as when a required field is missing, or data doesn't match the expected format.

3. **Authentication and Authorization Errors**: These occur when the user is not authenticated or does not have sufficient permissions.

---

### Built-in DRF Exception Classes

1. **APIException**:

   * The base class for all exceptions raised in DRF.
   * This class includes a default status code (500) and a default detail message, but it can be extended to customize the error message and HTTP status code.

   **Example**:

   ```python
   from rest_framework.exceptions import APIException

   class CustomException(APIException):
       status_code = 400
       default_detail = 'A custom error occurred.'
       default_code = 'custom_error'
   ```

2. **ValidationError**:

   * Raised when there are errors in request data validation, either during parsing or serialization.
   * DRF will automatically return a `400 Bad Request` response with the validation errors.

   **Example**:

   ```python
   from rest_framework.exceptions import ValidationError

   def validate_data(data):
       if data.get('age') < 18:
           raise ValidationError("Age must be at least 18.")
   ```

3. **NotFound**:

   * Raised when a resource cannot be found. Typically, this corresponds to a `404 Not Found` HTTP status.

   **Example**:

   ```python
   from rest_framework.exceptions import NotFound

   raise NotFound("The requested resource does not exist.")
   ```

4. **PermissionDenied**:

   * Raised when a user is attempting to access a resource they do not have permission to view, resulting in a `403 Forbidden` HTTP status.

   **Example**:

   ```python
   from rest_framework.exceptions import PermissionDenied

   raise PermissionDenied("You do not have permission to access this resource.")
   ```

5. **AuthenticationFailed**:

   * Raised when authentication fails, resulting in a `401 Unauthorized` HTTP status.

   **Example**:

   ```python
   from rest_framework.exceptions import AuthenticationFailed

   raise AuthenticationFailed("Authentication credentials were not provided.")
   ```

6. **ParseError**:

   * Raised when the data cannot be parsed (e.g., invalid JSON format), resulting in a `400 Bad Request` response.

   **Example**:

   ```python
   from rest_framework.exceptions import ParseError

   raise ParseError("Invalid JSON format.")
   ```

7. **MethodNotAllowed**:

   * Raised when a HTTP method (e.g., GET, POST) is not allowed for a resource. This results in a `405 Method Not Allowed` response.

   **Example**:

   ```python
   from rest_framework.exceptions import MethodNotAllowed

   raise MethodNotAllowed("GET method is not allowed for this resource.")
   ```

---

### Handling Exceptions in Views

DRF automatically handles exceptions raised in views and returns appropriate HTTP responses, but developers can also manually handle exceptions.

#### Using `try-except` in Views:

You can catch exceptions in views and return custom error messages or HTTP status codes.

**Example**:

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.exceptions import ValidationError

class MyView(APIView):
    def post(self, request):
        try:
            # Simulate some validation logic
            if not request.data.get('username'):
                raise ValidationError('Username is required.')
            return Response({'message': 'Data received successfully.'})
        except ValidationError as e:
            return Response({'error': str(e)}, status=400)
```

---

### Customizing Exception Handling Globally

You can globally customize exception handling in DRF by overriding the `EXCEPTION_HANDLER` setting in the `settings.py` file. By default, DRF uses `rest_framework.exceptions.exception_handler`, but you can implement your own custom exception handler.

#### Custom Exception Handler Example:

```python
from rest_framework.views import exception_handler

def custom_exception_handler(exc, context):
    # Call DRF's default exception handler to get the standard error response
    response = exception_handler(exc, context)

    if response is not None:
        # Customize the response data here if needed
        response.data['custom_error'] = 'Additional error details here.'

    return response
```

In `settings.py`, set the custom handler:

```python
REST_FRAMEWORK = {
    'EXCEPTION_HANDLER': 'myapp.utils.custom_exception_handler',
}
```

---

### Exception Handling with Viewsets

You can also catch exceptions in viewsets. By default, DRF handles exceptions in `APIView` and `ModelViewSet` automatically, but custom behavior can be defined.

#### Example of Handling Exceptions in Viewsets:

```python
from rest_framework import viewsets
from rest_framework.exceptions import NotFound

class MyViewSet(viewsets.ViewSet):
    def retrieve(self, request, pk=None):
        try:
            # Assume `get_object` fetches a model object based on the primary key
            instance = get_object(pk)
            return Response({'data': instance})
        except SomeModel.DoesNotExist:
            raise NotFound("Object not found.")
```

---

### Custom Exception Responses

When creating custom exceptions, you can define:

* `status_code`: The HTTP status code returned with the exception.
* `default_detail`: The default error message for the exception.
* `default_code`: A machine-readable code for the exception.

**Example**:

```python
from rest_framework.exceptions import APIException

class CustomError(APIException):
    status_code = 400
    default_detail = 'A custom error occurred.'
    default_code = 'custom_error'
```

---

### Summary of Key Concepts

* **Exception Handling**: A mechanism to gracefully handle errors and return meaningful error messages with appropriate HTTP status codes.
* **APIException**: The base class for all DRF exceptions.
* **ValidationError**: Raised when input validation fails.
* **AuthenticationFailed**: Raised when authentication credentials are invalid or missing.
* **NotFound**: Raised when a resource is not found.
* **PermissionDenied**: Raised when a user lacks permission to access a resource.
* **Custom Exception Handler**: Allows global customization of how exceptions are handled in DRF.

---

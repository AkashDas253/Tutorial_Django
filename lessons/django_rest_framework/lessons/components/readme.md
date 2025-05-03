## **`rest_framework` Modules and Submodules:**

- **`# Use rest_framework`**
  - `rest_framework.views.APIView` # Base class for creating class-based views for APIs.
  - `rest_framework.response.Response` # Used to return data in response to an API request.
  - `rest_framework.decorators.api_view` # Decorator to define function-based views as API views.
  - `rest_framework.parsers.JSONParser` # Parses incoming JSON data.
  - `rest_framework.parsers.FormParser` # Parses incoming form data.
  - `rest_framework.parsers.MultiPartParser` # Parses incoming multipart form data.
  - `rest_framework.renderers.JSONRenderer` # Renders response data as JSON.
  - `rest_framework.renderers.XMLRenderer` # Renders response data as XML.
  - `rest_framework.renderers.BrowsableAPIRenderer` # Renders a browsable API interface.
  - `rest_framework.exceptions.APIException` # Base class for DRF exceptions.

- **`# Use rest_framework.generics`**
  - `rest_framework.generics.ListAPIView` # Provides a read-only list of model instances.
  - `rest_framework.generics.RetrieveAPIView` # Provides read-only access to a single model instance.
  - `rest_framework.generics.CreateAPIView` # Provides a view to create a new model instance.
  - `rest_framework.generics.UpdateAPIView` # Provides a view to update an existing model instance.
  - `rest_framework.generics.DestroyAPIView` # Provides a view to delete a model instance.
  - `rest_framework.generics.ListCreateAPIView` # Combines list and create view functionality.
  - `rest_framework.generics.RetrieveUpdateAPIView` # Combines retrieve and update view functionality.
  - `rest_framework.generics.RetrieveDestroyAPIView` # Combines retrieve and destroy view functionality.
  - `rest_framework.generics.ListAPIView` # List all objects for the model.
  - `rest_framework.generics.UpdateAPIView` # View for updating an object.

- **`# Use rest_framework.mixins`**
  - `rest_framework.mixins.CreateModelMixin` # Provides `create()` action in viewsets.
  - `rest_framework.mixins.ListModelMixin` # Provides `list()` action in viewsets.
  - `rest_framework.mixins.UpdateModelMixin` # Provides `update()` action in viewsets.
  - `rest_framework.mixins.RetrieveModelMixin` # Provides `retrieve()` action in viewsets.
  - `rest_framework.mixins.DestroyModelMixin` # Provides `destroy()` action in viewsets.

- **`# Use rest_framework.viewsets`**
  - `rest_framework.viewsets.ModelViewSet` # Combines all standard actions (CRUD) in a single class.
  - `rest_framework.viewsets.ReadOnlyModelViewSet` # Provides a read-only model viewset.
  - `rest_framework.viewsets.GenericViewSet` # Base class for viewsets using mixins.

- **`# Use rest_framework.serializers`**
  - `rest_framework.serializers.Serializer` # Base class for DRF serializers.
  - `rest_framework.serializers.ModelSerializer` # Serializer class to easily create serializers from models.
  - `rest_framework.serializers.CharField` # Serializer field for strings.
  - `rest_framework.serializers.IntegerField` # Serializer field for integers.
  - `rest_framework.serializers.DateTimeField` # Serializer field for datetime.
  - `rest_framework.serializers.BooleanField` # Serializer field for boolean values.
  - `rest_framework.serializers.FloatField` # Serializer field for floating point values.
  - `rest_framework.serializers.ListField` # Serializer field for lists.
  - `rest_framework.serializers.DictField` # Serializer field for dictionaries.
  - `rest_framework.serializers.ValidationError` # Exception raised for validation errors.

- **`# Use rest_framework.authentication`**
  - `rest_framework.authentication.BaseAuthentication` # Base class for custom authentication.
  - `rest_framework.authentication.SessionAuthentication` # Authentication using Django sessions.
  - `rest_framework.authentication.BasicAuthentication` # Basic authentication class.
  - `rest_framework.authentication.TokenAuthentication` # Token-based authentication.

- **`# Use rest_framework.permissions`**
  - `rest_framework.permissions.AllowAny` # Permission class that allows any access.
  - `rest_framework.permissions.IsAuthenticated` # Permission class that allows authenticated users.
  - `rest_framework.permissions.IsAdminUser` # Permission class that allows admin users.
  - `rest_framework.permissions.IsAuthenticatedOrReadOnly` # Permission class that allows authenticated users to perform all actions and others to view only.
  - `rest_framework.permissions.DjangoModelPermissions` # Permission class that grants permissions based on Django model permissions.
  - `rest_framework.permissions.DjangoObjectPermissions` # Permission class that grants permissions based on object-level model permissions.

- **`# Use rest_framework.throttling`**
  - `rest_framework.throttling.BaseThrottle` # Base class for custom throttling.
  - `rest_framework.throttling.UserRateThrottle` # Throttling based on user rate.
  - `rest_framework.throttling.AnonRateThrottle` # Throttling based on anonymous user rate.

- **`# Use rest_framework.pagination`**
  - `rest_framework.pagination.PageNumberPagination` # Paginates results based on page numbers.
  - `rest_framework.pagination.LimitOffsetPagination` # Paginates results based on limit and offset.
  - `rest_framework.pagination.CursorPagination` # Paginates results based on a cursor for pagination.

- **`# Use rest_framework.renderers`**
  - `rest_framework.renderers.JSONRenderer` # Renders response data as JSON.
  - `rest_framework.renderers.XMLRenderer` # Renders response data as XML.
  - `rest_framework.renderers.BrowsableAPIRenderer` # Renders a browsable API interface.

- **`# Use rest_framework.parsers`**
  - `rest_framework.parsers.JSONParser` # Parses incoming JSON data.
  - `rest_framework.parsers.FormParser` # Parses incoming form data.
  - `rest_framework.parsers.MultiPartParser` # Parses incoming multipart form data.

- **`# Use rest_framework.status`**
  - `rest_framework.status.HTTP_200_OK` # HTTP 200 OK status code.
  - `rest_framework.status.HTTP_201_CREATED` # HTTP 201 Created status code.
  - `rest_framework.status.HTTP_204_NO_CONTENT` # HTTP 204 No Content status code.
  - `rest_framework.status.HTTP_400_BAD_REQUEST` # HTTP 400 Bad Request status code.
  - `rest_framework.status.HTTP_401_UNAUTHORIZED` # HTTP 401 Unauthorized status code.
  - `rest_framework.status.HTTP_403_FORBIDDEN` # HTTP 403 Forbidden status code.
  - `rest_framework.status.HTTP_404_NOT_FOUND` # HTTP 404 Not Found status code.
  - `rest_framework.status.HTTP_500_INTERNAL_SERVER_ERROR` # HTTP 500 Internal Server Error status code.

- **`# Use rest_framework.utils`**
  - `rest_framework.utils.serializer_helpers.ReturnList` # Custom list to handle serialized data.
  - `rest_framework.utils.serializer_helpers.ReturnDict` # Custom dictionary to handle serialized data.

- **`# Use rest_framework.filters`**
  - `rest_framework.filters.OrderingFilter` # Allows filtering results by ordering.
  - `rest_framework.filters.SearchFilter` # Allows searching through the results.
  - `rest_framework.filters.DjangoFilterBackend` # Integrates Django filter sets with DRF.

---
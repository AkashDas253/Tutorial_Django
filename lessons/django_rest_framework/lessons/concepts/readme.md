## Django REST Framework (DRF)

### Core Concepts

- Serialization

  - Serializer
  - ModelSerializer
  - HyperlinkedModelSerializer
  - Custom serializer fields
  - Validation methods (`validate`, `validate_<field>`)

- Views

  - APIView
  - GenericAPIView
  - Mixins (ListModelMixin, CreateModelMixin, etc.)
  - ViewSet
  - ModelViewSet
  - ReadOnlyModelViewSet
  - Custom ViewSet

- Routers

  - SimpleRouter
  - DefaultRouter
  - Custom routers

- Request and Response

  - Request object
  - Response object
  - `.data`, `.query_params`, `.user`, `.auth`
  - Status module (`rest_framework.status`)

### Authentication

- SessionAuthentication
- BasicAuthentication
- TokenAuthentication
- JWTAuthentication (with third-party packages)
- Custom authentication classes

### Permissions

- AllowAny
- IsAuthenticated
- IsAdminUser
- IsAuthenticatedOrReadOnly
- Object-level permissions
- Custom permission classes

### Throttling

- AnonRateThrottle
- UserRateThrottle
- Custom throttle classes
- Settings: `DEFAULT_THROTTLE_CLASSES`, `DEFAULT_THROTTLE_RATES`

### Pagination

- PageNumberPagination
- LimitOffsetPagination
- CursorPagination
- Custom pagination classes
- Settings: `DEFAULT_PAGINATION_CLASS`, `PAGE_SIZE`

### Filtering

- DjangoFilterBackend
- SearchFilter
- OrderingFilter
- Custom filters
- URL query parameters: `?search=`, `?ordering=`, `?field=value`

### Versioning

- NamespaceVersioning
- URLPathVersioning
- HostNameVersioning
- AcceptHeaderVersioning
- Custom versioning classes

### Parsers and Renderers

- Parsers

  - JSONParser
  - FormParser
  - MultiPartParser

- Renderers

  - JSONRenderer
  - BrowsableAPIRenderer
  - Custom renderers

### Content Negotiation

- BaseContentNegotiation
- Custom negotiation classes

### Exception Handling

- APIException
- ValidationError
- Custom exception handling
- Custom `exception_handler`

### Schema and Documentation

- CoreAPI (deprecated), OpenAPI
- SchemaGenerator
- `get_schema_view()`
- Swagger and Redoc via third-party packages

### Testing

- APITestCase
- APIClient
- HTTP method functions: `get`, `post`, `put`, `patch`, `delete`
- Token and session handling in tests

### Browsable API

- HTML form display
- Pagination and filter UI support
- Authentication via the browsable interface

### ViewSet Actions

- Extra actions using method definitions
- Parameters: `detail`, `url_path`, `url_name`
- Custom endpoints within ViewSets

### Signals Integration

- Django signals like `pre_save`, `post_save`
- Signal usage with DRF models and serializers

### Nested Serializers

- Nested/related serializers
- `depth` in Meta class
- `SerializerMethodField`

### File Upload Handling

- FileField and ImageField in serializers
- Use of MultiPartParser
- File storage configuration

### Caching

- Per-view caching
- Schema caching
- Use of `cache_response` from `rest_framework_extensions`

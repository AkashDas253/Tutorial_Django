## ViewSet Actions in Django REST Framework (DRF)

In Django REST Framework (DRF), **ViewSets** are a high-level abstraction that automatically handle CRUD (Create, Read, Update, Delete) operations for your models. **Actions** refer to the specific operations or methods that can be performed on the resources managed by the ViewSet. DRF provides a set of pre-defined actions and allows for customization of these actions to suit the requirements of the API.

### Key Concepts in ViewSet Actions

1. **Default Actions**:

   * DRF's `ViewSet` provides automatic implementations for standard actions like `list`, `create`, `retrieve`, `update`, and `destroy` based on the HTTP methods (GET, POST, PUT, DELETE).

   * These actions correspond to the CRUD operations:

     * `list`: Retrieves a list of resources (HTTP GET).
     * `create`: Creates a new resource (HTTP POST).
     * `retrieve`: Retrieves a single resource (HTTP GET).
     * `update`: Updates an existing resource (HTTP PUT).
     * `destroy`: Deletes a resource (HTTP DELETE).

2. **Custom Actions**:

   * DRF also allows you to define **custom actions** in your ViewSets for operations that don't fit within the standard CRUD operations.
   * Custom actions are methods defined within the ViewSet and are linked to HTTP methods like GET, POST, or others.
   * You can use the `@action` decorator to define custom actions.

### Built-In Actions in ViewSets

1. **List**:

   * The `list` action is automatically provided by DRF's `ModelViewSet` for listing resources.
   * Corresponds to the `GET` request on the collection endpoint.

   Example:

   ```python
   @action(detail=False, methods=['get'])
   def list(self, request):
       queryset = self.get_queryset()
       serializer = self.get_serializer(queryset, many=True)
       return Response(serializer.data)
   ```

2. **Create**:

   * The `create` action is automatically provided by DRF’s `ModelViewSet` for creating resources.
   * Corresponds to the `POST` request to create a new resource.

   Example:

   ```python
   @action(detail=False, methods=['post'])
   def create(self, request):
       serializer = self.get_serializer(data=request.data)
       if serializer.is_valid():
           serializer.save()
           return Response(serializer.data, status=status.HTTP_201_CREATED)
       return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
   ```

3. **Retrieve**:

   * The `retrieve` action is automatically provided by DRF’s `ModelViewSet` for retrieving a single resource.
   * Corresponds to the `GET` request on an individual resource.

   Example:

   ```python
   @action(detail=True, methods=['get'])
   def retrieve(self, request, pk=None):
       instance = self.get_object()
       serializer = self.get_serializer(instance)
       return Response(serializer.data)
   ```

4. **Update**:

   * The `update` action is automatically provided by DRF’s `ModelViewSet` for updating an existing resource.
   * Corresponds to the `PUT` request to update a resource.

   Example:

   ```python
   @action(detail=True, methods=['put'])
   def update(self, request, pk=None):
       instance = self.get_object()
       serializer = self.get_serializer(instance, data=request.data)
       if serializer.is_valid():
           serializer.save()
           return Response(serializer.data)
       return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
   ```

5. **Destroy**:

   * The `destroy` action is automatically provided by DRF’s `ModelViewSet` for deleting a resource.
   * Corresponds to the `DELETE` request to delete a resource.

   Example:

   ```python
   @action(detail=True, methods=['delete'])
   def destroy(self, request, pk=None):
       instance = self.get_object()
       instance.delete()
       return Response(status=status.HTTP_204_NO_CONTENT)
   ```

---

### Custom Actions in ViewSets

In addition to the default actions, you can create **custom actions** using the `@action` decorator. These custom actions are useful when you need to perform additional operations that don't directly correspond to CRUD actions.

1. **Creating Custom Actions**:

   * To create a custom action, use the `@action` decorator on the method you want to expose.
   * The `detail` argument specifies whether the action applies to a single resource (`True`) or a list of resources (`False`).
   * The `methods` argument defines the allowed HTTP methods (e.g., `GET`, `POST`, `PUT`, `DELETE`).

   Example of a custom action:

   ```python
   from rest_framework.decorators import action
   from rest_framework.response import Response

   class ProductViewSet(viewsets.ModelViewSet):
       queryset = Product.objects.all()
       serializer_class = ProductSerializer

       @action(detail=True, methods=['post'])
       def mark_as_featured(self, request, pk=None):
           product = self.get_object()
           product.is_featured = True
           product.save()
           return Response({'status': 'product marked as featured'})
   ```

   In this example, a custom `mark_as_featured` action is added to the `ProductViewSet`. This action is accessible via a `POST` request for a specific product (`detail=True`).

2. **Custom Action with Parameters**:

   * Custom actions can also accept additional parameters passed in the URL, which can be accessed in the view method.

   Example:

   ```python
   @action(detail=False, methods=['get'])
   def search(self, request):
       query = request.query_params.get('q', '')
       results = Product.objects.filter(name__icontains=query)
       serializer = self.get_serializer(results, many=True)
       return Response(serializer.data)
   ```

3. **Action Response**:

   * Actions should return a **Response** object, which is DRF’s way of handling the HTTP response for the API.

4. **Custom URL Patterns**:

   * You can also define custom URL patterns for your actions. DRF’s router system automatically generates routes for ViewSets, but for custom actions, you may need to manually define URLs.

   Example:

   ```python
   from rest_framework.routers import DefaultRouter

   router = DefaultRouter()
   router.register(r'products', ProductViewSet)
   urlpatterns = router.urls
   ```

---

### Conclusion

**ViewSet Actions** in Django REST Framework provide a powerful and flexible way to define the operations available for your resources. The built-in CRUD actions (`list`, `create`, `retrieve`, `update`, `destroy`) cover the most common operations, while **custom actions** allow you to define additional operations specific to your needs. These custom actions are easily created using the `@action` decorator and can be configured to handle various HTTP methods and parameters.

---

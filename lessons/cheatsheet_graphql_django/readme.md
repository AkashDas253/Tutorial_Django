### **GraphQL with Django Cheatsheet**  

GraphQL allows clients to fetch only the data they need with a single query. **Graphene-Django** is the main library for integrating GraphQL with Django.  

---

## **1. Installation & Setup**  

### **Install Graphene-Django**  
```sh
pip install graphene-django
```

### **Add to `INSTALLED_APPS` (`settings.py`)**  
```python
INSTALLED_APPS = [
    'graphene_django',
]
```

### **Configure GraphQL in `settings.py`**  
```python
GRAPHENE = {
    "SCHEMA": "myapp.schema.schema"  # Replace with your schema path
}
```

---

## **2. Defining a Schema**  

### **Create Schema File (`schema.py`)**  
```python
import graphene
from graphene_django.types import DjangoObjectType
from .models import Book

class BookType(DjangoObjectType):
    class Meta:
        model = Book

class Query(graphene.ObjectType):
    all_books = graphene.List(BookType)

    def resolve_all_books(self, info):
        return Book.objects.all()

schema = graphene.Schema(query=Query)
```

| **GraphQL Component** | **Description** |
|-----------------|-----------------|
| `DjangoObjectType` | Converts a Django model into a GraphQL type. |
| `Query` | Defines how data can be queried. |
| `resolve_*` | Method that fetches data. |

---

## **3. Setting Up a GraphQL Endpoint**  

### **Define URL Pattern (`urls.py`)**  
```python
from django.urls import path
from graphene_django.views import GraphQLView
from myapp.schema import schema

urlpatterns = [
    path("graphql/", GraphQLView.as_view(graphiql=True, schema=schema)),
]
```

| **Option** | **Description** |
|-----------|----------------|
| `graphiql=True` | Enables GraphQL UI at `/graphql/`. |
| `schema=schema` | Connects to the defined GraphQL schema. |

---

## **4. Running a GraphQL Query**  

### **Example Query**  
```graphql
{
  allBooks {
    title
    author
    publishedDate
  }
}
```

| **Feature** | **Description** |
|------------|----------------|
| **No Over-fetching** | Only requested fields are returned. |
| **Single Endpoint** | No need for multiple REST API endpoints. |

---

## **5. Adding Mutations (Create, Update, Delete)**  

### **Define a Mutation (`schema.py`)**  
```python
class CreateBook(graphene.Mutation):
    class Arguments:
        title = graphene.String()
        author = graphene.String()
        published_date = graphene.String()

    book = graphene.Field(BookType)

    def mutate(self, info, title, author, published_date):
        book = Book(title=title, author=author, published_date=published_date)
        book.save()
        return CreateBook(book=book)

class Mutation(graphene.ObjectType):
    create_book = CreateBook.Field()

schema = graphene.Schema(query=Query, mutation=Mutation)
```

### **Example Mutation Request**  
```graphql
mutation {
  createBook(title: "Django GraphQL", author: "John Doe", publishedDate: "2025-01-01") {
    book {
      title
      author
    }
  }
}
```

| **Mutation Feature** | **Description** |
|----------------|----------------|
| **Arguments** | Data sent to the server. |
| **Mutation Class** | Defines what happens when the mutation is called. |

---

## **6. Using Variables in Queries**  

### **Query with Variables**  
```graphql
query GetBooks($author: String!) {
  allBooks(author: $author) {
    title
    publishedDate
  }
}
```

### **Sending Variables**  
```json
{
  "author": "John Doe"
}
```

| **Feature** | **Benefit** |
|------------|------------|
| **Variables** | Reuse queries with different values. |

---

## **7. Authentication & Permissions**  

### **Add Authentication to Queries**  
```python
from graphene_django.types import DjangoObjectType
from django.contrib.auth.models import User
from graphql_jwt.decorators import login_required

class UserType(DjangoObjectType):
    class Meta:
        model = User

class Query(graphene.ObjectType):
    me = graphene.Field(UserType)

    @login_required
    def resolve_me(self, info):
        return info.context.user
```

| **Feature** | **Benefit** |
|------------|------------|
| `@login_required` | Ensures only logged-in users access certain data. |

---

## **8. JWT Authentication**  

### **Install GraphQL JWT**  
```sh
pip install django-graphql-jwt
```

### **Update `settings.py`**  
```python
GRAPHENE = {
    "SCHEMA": "myapp.schema.schema",
    "MIDDLEWARE": [
        "graphql_jwt.middleware.JSONWebTokenMiddleware",
    ],
}

AUTHENTICATION_BACKENDS = [
    "graphql_jwt.backends.JSONWebTokenBackend",
    "django.contrib.auth.backends.ModelBackend",
]
```

### **Define JWT Mutations (`schema.py`)**  
```python
import graphene
import graphql_jwt

class Mutation(graphene.ObjectType):
    token_auth = graphql_jwt.ObtainJSONWebToken.Field()
    verify_token = graphql_jwt.Verify.Field()
    refresh_token = graphql_jwt.Refresh.Field()

schema = graphene.Schema(query=Query, mutation=Mutation)
```

### **Login Mutation Request**  
```graphql
mutation {
  tokenAuth(username: "john", password: "password123") {
    token
  }
}
```

| **Feature** | **Description** |
|------------|----------------|
| `tokenAuth` | Generates a JWT token. |
| `verifyToken` | Checks token validity. |
| `refreshToken` | Refreshes an expired token. |

---

## **9. Filtering with GraphQL**  

### **Install Django Filter**  
```sh
pip install django-filter
```

### **Enable Filtering (`schema.py`)**  
```python
from graphene_django.filter import DjangoFilterConnectionField

class Query(graphene.ObjectType):
    all_books = DjangoFilterConnectionField(BookType)
```

### **GraphQL Query with Filtering**  
```graphql
{
  allBooks(title_Icontains: "Django") {
    edges {
      node {
        title
        author
      }
    }
  }
}
```

| **Feature** | **Description** |
|------------|----------------|
| **DjangoFilterConnectionField** | Enables GraphQL filtering. |

---

# Learning Django

This repository is meant to contain the learning and practice done using django.

## Content

- [Django Basics](lessons/basics/readme.md)
- [Concepts](lessons/concepts/readme.md)
- [Components](lessons/components/readme.md)



- [Core of Django](lessons/core/readme.md)

- Models (Database Layer)

  - [**Model Definition**](lessons/model_creation/readme.md)
  - [**Model Relationships**](lessons/model_relationships/readme.md) 
  - [**Model Queries (ORM)**](lessons/model_query/readme.md)
  - [**Database Migrations**](lessons/database_migration/readme.md)  
  - [**Model Methods** ](lessons/model_methods/readme.md) 

- Views (Business Logic Layer)
  - [Function-Based Views (FBVs)](lessons/fbv/readme.md)
        - [Using HTTP methods](lessons/fbv_hhtp_methods/readme.md) 
        - [Decorators for FBVs](lessons/fbv_decorator/readme.md)
  - [Class-Based Views (CBVs)](lessons/cbv/readme.md)
    - [Types](lessons/cbv_types/readme.md)
        - Generic Views
          - `TemplateView`
          - `RedirectView`
        - Detail Views
          - `DetailView`
        - List Views
          - `ListView`
        - Form Views
          - `FormView`
        - CRUD Operations
          - `CreateView`
          - `UpdateView`
          - `DeleteView`
      - **Mixin Views**
        - `LoginRequiredMixin`
        - `PermissionRequiredMixin`
        - `ContextMixin`


- Third-Party Tools and Integrations
  - [Django REST Framework (DRF)](lessons/drf/readme.md)

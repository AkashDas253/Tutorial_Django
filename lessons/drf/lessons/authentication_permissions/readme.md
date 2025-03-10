## **Overview of Authentication & Permissions in Django Rest Framework (DRF)**  

### **Concept and Purpose**  
Authentication identifies **who** is making the request, while permissions control **what** an authenticated user can access. DRF provides built-in authentication methods and customizable permission classes to secure API endpoints.  

---

### **Authentication in DRF**  

| Authentication Type  | Description |
|---------------------|-------------|
| **Session Authentication**  | Uses Django sessions for authentication (for web apps). |
| **Basic Authentication**  | Uses username-password but sends credentials with every request (not secure for production). |
| **Token Authentication**  | Assigns a token to each user for API access (stateless authentication). |
| **JWT Authentication**  | Uses JSON Web Tokens (JWT) for secure and scalable authentication. |
| **OAuth2 Authentication**  | Enables third-party authentication (Google, GitHub, etc.). |
| **Custom Authentication**  | Allows defining custom authentication logic. |

- Authentication is defined in `settings.py` under `DEFAULT_AUTHENTICATION_CLASSES`.  
- Token and JWT authentication are common for stateless API access.  

---

### **Permissions in DRF**  

| Permission Type      | Description |
|---------------------|-------------|
| `AllowAny`          | No restrictions (public access). |
| `IsAuthenticated`   | Grants access only to authenticated users. |
| `IsAdminUser`       | Grants access only to admin users. |
| `IsAuthenticatedOrReadOnly` | Allows full access to authenticated users, read-only for others. |
| `DjangoModelPermissions` | Uses Django’s built-in model permissions. |
| `Custom Permissions` | Custom logic for access control. |

- Permissions are set per view using the `permission_classes` attribute.  
- Can restrict access based on user roles, ownership, or object-level rules.  

---

### **Custom Authentication & Permissions**  
- **Custom Authentication**: Define logic for token validation, user identification, or external API authentication.  
- **Custom Permissions**: Implement object-level access control based on request attributes or user roles.  

---

### **Authentication vs. Authorization**  

| Aspect         | Authentication | Permissions (Authorization) |
|--------------|----------------|-----------------------------|
| Purpose      | Identifies users | Determines user access levels |
| Scope        | API-wide (user identity) | View-level (access control) |
| Examples     | Token authentication, JWT | `IsAuthenticated`, `IsAdminUser` |

---

### **Best Practices**  
- Use **Token/JWT Authentication** for APIs instead of session-based authentication.  
- Combine authentication with **permissions** for secure API design.  
- Implement **custom permissions** for fine-grained access control.  
- Secure sensitive endpoints with **IsAuthenticated** or **IsAdminUser**.  

---

### **Conclusion**  
Authentication ensures users are verified, while permissions define their access. DRF provides multiple authentication methods and permission classes to enforce security, supporting both built-in and custom implementations.
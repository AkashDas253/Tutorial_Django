## **Overview of Pagination in Django Rest Framework (DRF)**  

### **Concept and Purpose**  
- Pagination controls the number of records returned per API request.  
- Improves performance by reducing data load.  
- Enhances user experience by making large datasets more manageable.  

---

### **Types of Pagination in DRF**  

| Pagination Type | Description |
|----------------|-------------|
| **Page Number Pagination** | Divides data into numbered pages (`?page=2`). |
| **Limit Offset Pagination** | Uses `limit` (items per request) and `offset` (starting point) (`?limit=5&offset=10`). |
| **Cursor Pagination** | Uses an encoded cursor for efficient navigation (`?cursor=cD0yMDIzLTAzLTAx`). |
| **Custom Pagination** | Allows defining custom rules for paginating API responses. |

---

### **Global Pagination Configuration**  
Set default pagination in `settings.py`:  
```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10
}
```
- Limits responses to **10 items per page** by default.  
- Can be overridden per view if needed.  

---

### **Best Practices**  
- Use **Page Number Pagination** for simple navigation.  
- Use **Limit Offset Pagination** for flexibility with large datasets.  
- Use **Cursor Pagination** for efficient sorting and retrieval.  
- Customize pagination as needed for business-specific requirements.  

---

### **Conclusion**  
Pagination ensures APIs remain performant and scalable by limiting response sizes while allowing flexible data retrieval.
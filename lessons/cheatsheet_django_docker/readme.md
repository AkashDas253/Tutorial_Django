### **Django & Docker Cheatsheet**  

Docker allows you to containerize Django applications for easy deployment and scalability.  

---

## **1. Install Docker**  
- **Ubuntu**: `sudo apt install docker docker-compose`  
- **macOS**: `brew install --cask docker`  
- **Windows**: Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)  

---

## **2. Create `Dockerfile`**  

```dockerfile
# Base image
FROM python:3.11

# Set working directory
WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy project files
COPY . .

# Expose port
EXPOSE 8000

# Run Django server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

---

## **3. Create `docker-compose.yml`**  

```yaml
version: '3'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - .:/app
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

---

## **4. Build & Run the Containers**  

```sh
docker-compose up --build
```

---

## **5. Manage Containers**  

- **Stop Containers**: `docker-compose down`  
- **Rebuild & Restart**: `docker-compose up --build -d`  
- **View Running Containers**: `docker ps`  
- **Enter Django Container**: `docker exec -it <container_id> bash`  

---

## **6. Run Django Commands Inside Docker**  

```sh
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
```

---

## **7. Serve Static Files in Production (`settings.py`)**  

```python
STATIC_ROOT = "/app/static"
STATIC_URL = "/static/"
```

**Modify `Dockerfile` to Collect Static Files**
```dockerfile
RUN python manage.py collectstatic --noinput
```

---

## **8. Deploying with Gunicorn**  

### **Update `Dockerfile`**
```dockerfile
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "myproject.wsgi:application"]
```

---

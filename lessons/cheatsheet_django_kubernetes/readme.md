### **Django & Kubernetes Cheatsheet**  

Kubernetes (K8s) helps manage and deploy Django applications in a scalable way using container orchestration.  

---

## **1. Prerequisites**  
- Install **Docker** & **Kubernetes** (Minikube for local testing)  
- Install **kubectl** (`brew install kubectl` / `sudo apt install kubectl`)  
- Install **Helm** (`brew install helm` / `sudo apt install helm`)  

---

## **2. Create `Dockerfile`**  

```dockerfile
FROM python:3.11

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["gunicorn", "--bind", "0.0.0.0:8000", "myproject.wsgi:application"]
```

### **Build & Push Image to Docker Hub**  
```sh
docker build -t username/django-app .
docker push username/django-app
```

---

## **3. Create Kubernetes Deployment & Service**  

### **Create `deployment.yaml`**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: django-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: django
  template:
    metadata:
      labels:
        app: django
    spec:
      containers:
        - name: django
          image: username/django-app:latest
          ports:
            - containerPort: 8000
          env:
            - name: DJANGO_SETTINGS_MODULE
              value: "myproject.settings"
```

---

### **Create `service.yaml`**  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: django-service
spec:
  selector:
    app: django
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8000
  type: LoadBalancer
```

---

## **4. Deploy Django App to Kubernetes**  

```sh
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## **5. Expose the Service**  
```sh
kubectl get services
minikube service django-service
```

---

## **6. Add PostgreSQL to Kubernetes**  

### **Create `postgres-deployment.yaml`**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
spec:
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:15
          env:
            - name: POSTGRES_DB
              value: mydb
            - name: POSTGRES_USER
              value: user
            - name: POSTGRES_PASSWORD
              value: password
          ports:
            - containerPort: 5432
```

### **Create `postgres-service.yaml`**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres-service
spec:
  selector:
    app: postgres
  ports:
    - protocol: TCP
      port: 5432
      targetPort: 5432
```

### **Deploy PostgreSQL**
```sh
kubectl apply -f postgres-deployment.yaml
kubectl apply -f postgres-service.yaml
```

---

## **7. Scaling Django Application**
```sh
kubectl scale deployment django-app --replicas=5
```

---

## **8. View Logs & Debugging**
```sh
kubectl get pods
kubectl logs <pod_name>
kubectl describe pod <pod_name>
kubectl delete pod <pod_name>
```

---

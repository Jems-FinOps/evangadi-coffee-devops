# ☕ Evangadi Coffee – DevOps Project

## 📌 Overview
This project demonstrates a complete DevOps workflow by deploying a React application using Docker and Kubernetes (Minikube).

## 🛠️ Tech Stack
- Frontend: React (Vite)
- Containerization: Docker
- Orchestration: Kubernetes (Minikube)
- Monitoring: Grafana + Prometheus

## ⚙️ Features
- Dockerized React application
- Kubernetes Deployment & Service
- NodePort exposure
- Scaling & rolling updates
- Monitoring with Grafana

evangadi-coffee/
├── src/
├── public/
├── Dockerfile
├── package.json
├── README.md
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
└── docs/
## 🚀 Deployment Steps

### 1. Build Docker Image
```bash
docker build -t evangadi-coffee 


## 📷 Verification

```bash
kubectl get pods
kubectl get svc
curl http://localhost:8081.



---

# 🧠 3. Architecture Section

## 🏗️ Architecture

User → Kubernetes Service → Pod → React App (Nginx)

- Docker container serves static React build
- Kubernetes manages scaling and networking


## ⚡ Useful Commands

```bash
kubectl get pods
kubectl get svc
kubectl describe pod <pod>
kubectl logs <pod>



---

# 🔥 Kubernetes YAML


---

## 📄 `k8s/deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: coffee-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: coffee-app
  template:
    metadata:
      labels:
        app: coffee-app
    spec:
      containers:
      - name: coffee-app
        image: evangadi-coffee
        imagePullPolicy: Never
        ports:
        - containerPort: 80

apiVersion: v1
kind: Service
metadata:
  name: coffee-app
spec:
  type: NodePort
  selector:
    app: coffee-app
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: coffee-ingress
spec:
  rules:
  - host: coffee.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: coffee-app
            port:
              number: 80


kubectl delete deployment coffee-app
kubectl delete svc coffee-app

kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

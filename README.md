# K8S-3-Tier-Project

# 🦸 Avengers API – Node.js | Docker | Kubernetes | Traefik

A simple **3-tier Kubernetes application** built using **Node.js**, **MongoDB**, **Docker**, and exposed via **Traefik Ingress Controller**.

This project demonstrates real-world DevOps concepts including containerization, service discovery, ingress routing, and deployment strategies.

---

## 📌 Architecture Overview

Client (Postman / Browser)  
→ Traefik LoadBalancer  
→ Traefik IngressRoute  
→ Node.js API Service  
→ Node.js Pods  
→ MongoDB Service  
→ MongoDB Pod  

---

## 🧩 Tech Stack

- Node.js (Express)
- MongoDB
- Docker
- Kubernetes
- Traefik Ingress Controller
- Postman for API testing

---

## 📂 Repository Structure
```text
.
├── server.js
├── package.json
├── Dockerfile
├── k8s/
│ ├── namespace.yaml
│ ├── mongo-deployment.yaml
│ ├── mongo-service.yaml
│ ├── app-deployment.yaml
│ ├── app-service.yaml
│ └── ingressroute.yaml
└── README.md
```

---

## 🚀 Application Features

- Add Avengers with name and power
- Fetch all Avengers
- Health check endpoint
- Horizontally scalable Node.js pods
- Traffic routed via Traefik

---

## 🔌 API Endpoints

### Health Check
GET /health


### Add Avenger
POST /avengers

**Request Body**
```json
{
  "name": "Iron Man",
  "power": "Genius Suit"
}
```
Get All Avengers
GET /avengers
### 🐳 Docker Setup
Build Docker Image
```text
docker build -t avengers-api:v1 .
```
Push the image to docker hub and reuse it in the nodejs deployment yaml file.
### ☸️ Kubernetes Deployment
Create Namespace
```text
kubectl apply -f k8s/namespace.yaml
```
Deploy MongoDB
```text
kubectl apply -f k8s/mongo-deployment.yaml
kubectl apply -f k8s/mongo-service.yaml
```
Deploy Node.js Application
```text
kubectl apply -f k8s/app-deployment.yaml
kubectl apply -f k8s/app-service.yaml
```
### 🌐 Traefik IngressRoute
This project uses Traefik IngressRoute CRD instead of traditional Ingress.
```text
apiVersion: traefik.io/v1alpha1
kind: IngressRoute
metadata:
  name: avengers-route
  namespace: avengers
spec:
  entryPoints:
    - web
  routes:
    - match: PathPrefix(`/`)
      kind: Rule
      services:
        - name: avengers-api-service
          port: 80
```
Apply it:
```text
kubectl apply -f k8s/ingressroute.yaml
```
### 🧪 Testing with Postman
Use Traefik LoadBalancer IP:
```text
http://<TRAEFIK-LB-IP>/avengers
```
Example:
```text
http://3.90.62.196/avengers
```
Ensure Traefik service is exposed:
```text
kubectl get svc -n traefik
```
### 🧹 Cleanup
kubectl delete namespace avengers
🙌 Author Built and demonstrated as a live Kubernetes teaching project.

⭐ If this helped you learn Kubernetes, give the repo a star!

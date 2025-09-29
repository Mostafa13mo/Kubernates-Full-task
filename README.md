Kubernetes Full Task 🚀
This project demonstrates a complete Kubernetes setup on Minikube with multiple namespaces, Deployments, Services, Volumes, Secrets, ConfigMaps, Network Policies, and Node scheduling using Taints & Tolerations.

🛠️ Prerequisites
Minikube installed
kubectl installed
At least 2 nodes in Minikube
minikube start --nodes 2
📂 Namespaces

We use 3 namespaces to separate our applications:

fe → Frontend Nginx app

mongo-db → MongoDB database

mongo-express → Mongo Express web interface

Create them: kubectl create namespace fe kubectl create namespace mongo-db kubectl create namespace mongo-express

🚀 Deployments & Services 1️⃣ Frontend (Nginx)

Deployment with 2 replicas

Uses an emptyDir Volume to serve custom content

NodePort Service exposes app externally on port 80

Apply: kubectl apply -f frontend-deployment.yaml -n fe kubectl apply -f frontend-service.yaml -n fe

2️⃣ MongoDB (Database)

Deployment with 1 replica (mongo:latest)

Uses:

Secret for root username/password

PVC for data persistence at /data/db

ClusterIP Service for internal access on port 27017

Apply: kubectl apply -f mongodb-secret.yaml -n mongo-db kubectl apply -f mongodb-pv-pvc.yaml -n mongo-db kubectl apply -f mongodb-deployment.yaml -n mongo-db kubectl apply -f mongodb-service.yaml -n mongo-db

3️⃣ Mongo Express

Deployment with 1 replica (mongo-express:latest)

Uses:

ConfigMap for env variables

Talks to mongodb-service in mongo-db namespace

NodePort Service exposes UI on port 8081

Apply: kubectl apply -f mongo-express-configmap.yaml -n mongo-express kubectl apply -f mongo-express-deployment.yaml -n mongo-express kubectl apply -f mongo-express-service.yaml -n mongo-express

4️⃣ Network Policy

Restricts MongoDB access to only the mongo-express namespace

Apply: kubectl apply -f mongodb-networkpolicy.yaml -n mongo-db

⚙️ Taints & Tolerations

Step 1: Taint a Node kubectl taint nodes db=NoSchedule:NoSchedule

Step 2: Apply Toleration in MongoDB Deployment In mongodb-deployment.yaml: tolerations:

key: "db" operator: "Equal" value: "NoSchedule" effect: "NoSchedule"
🌐 Access Services

Frontend App (Nginx) minikube service frontend-service -n fe

Mongo Express minikube service mongo-express-service -n mongo-express

✅ Summary

This setup demonstrates:

Multi-namespace isolation

Secrets, ConfigMaps, Volumes

NodePort & ClusterIP services

NetworkPolicy for namespace-restricted access

Taints & Tolerations for scheduling

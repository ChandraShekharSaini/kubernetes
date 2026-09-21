# kubernetes-veeraops-ecommerce-microservices-app
/
This README provides end-to-end steps for installing **eksctl**, **kubectl**, **Docker**, **Ingress-Nginx**, **MariaDB**, **ArgoCD**, and creating necessary namespaces and database tables

## 📸 Architecture & Workflow Diagrams

### Workflow Diagram

![Workflow Diagram](p1.png)

![Workflow Diagram](p2.png)

## Prerequisites

- AWS Account with appropriate IAM permissions
- Linux/Unix environment or WSL2 on Windows
- Sufficient compute resources for EKS cluster

---

## Deployment Workflow

Follow these steps in order for successful deployment:

### Step 1: EKS Cluster Setup with terraform
- Create EKS with terraform files mention on eks terrafom dirictory 

### Step 2: EKS Client & Cluster Update
- Already eks clinet server is creted update your terraform keys 
- Update cluster configuration
- Verify cluster connectivity using `kubectl get nodes`

### Step 3: Configure GitHub Secrets
Store the following secrets in your GitHub repository settings:
- AWS Access Key ID
- AWS Secret Access Key
- AWS Account ID
- Git PAT (Personal Access Token)

---
## Installation Steps

### 4. Install Git

```bash
yum install git -y
```

### 5. Install Ingress-Nginx

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx
```

### 6. Install ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
kubectl get svc -n argocd
```

### 7. Create Application Namespace

```bash
kubectl create namespace microservices
```

### 8. Retrieve ArgoCD Initial Admin Password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
### Step 9: Clone Repository on server
```bash
git clone <your-repository-url>   
```
### rds using as a data base mens run the follwing  scripts
```
on backend dirictory run test.sql

sudo dnf install mariadb105-server -y

mysql -h rds-endpoint -u admin -p < test.sql
```
### Step 10 : Deploy Backend
```bash
cd k8s-argocd/backend
kubectl apply -f .
# Wait for Load Balancer to be assigned
kubectl get svc -n microservices
```
### Step 11: Deploy Frontend
```bash
cd ../frontend
kubectl apply -f .
```

### Step 12: Deploy EFK Stack (Elasticsearch, Fluent Bit, Kibana)
```bash
cd ../efk-stack
kubectl apply -f .
```

### Step 13: Install Grafana & Prometheus
```bash
cd ../../grafana-prometheous
# Follow installation commands in grafana-prometheous/README.md
```
### Step 14: Access the Application
- Get the Ingress Load Balancer URL:
```bash
kubectl get ingress -n google
```
- Access the application using the Ingress Load Balancer URL in your browser



## Summary of Key Components

| Component | Purpose |
|-----------|---------|
| **EKS** | Managed Kubernetes service on AWS |
| **RDS** | Managed relational database (MySQL/MariaDB) |
| **ArgoCD** | Continuous Deployment tool |
| **Ingress-Nginx** | Ingress controller for routing |
| **EFK Stack** | Logging and debugging (Elasticsearch, Fluent Bit, Kibana) |
| **Grafana** | Metrics visualization |
| **Prometheus** | Metrics collection and alerting |

---
## for rds 
- sudo dnf install mariadb105-server -y
- on backend dirictory run test.sql
- mysql -h rds-endpoint -u admin -p<veerasir> < test.sql


**Last Updated:** March 2026

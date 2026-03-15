# 🚀 Kubernetes Ingress Lab

A hands-on **Kubernetes Ingress Lab** demonstrating how to expose multiple services through a **single entry point using the NGINX Ingress Controller**.

This lab focuses on understanding how **Path-based Routing** and **Host-based Routing** work in real Kubernetes environments.

The project simulates a **production-like traffic routing architecture** where a single Ingress resource controls access to multiple backend services.

---

# 📊 Architecture Overview

![Ingress Architecture](./ingress-architecture.png)

The **NGINX Ingress Controller** acts as a **reverse proxy and load balancer**, routing incoming HTTP requests to the correct Kubernetes service.

### Components

| Component | Description |
|--------|-------------|
| Client | Sends HTTP requests using `curl` |
| Ingress Controller | NGINX-based reverse proxy that manages routing |
| Services | Kubernetes ClusterIP services exposing applications |
| Pods | Backend containers running the applications |

---

# 🔀 Routing Types

## 1️⃣ Path-Based Routing

Routes traffic based on the **URL path**.

Example routing table:

| Path | Service |
|-----|--------|
| `/` | webapp-service |
| `/api` | api-service |
| `/admin` | admin-service |

Example test command:

```bash
curl http://<INGRESS-IP>/api
2️⃣ Host-Based Routing

Routes traffic based on domain names or subdomains.

Example routing table:

Host	Service
myapp.local	webapp-svc
api.myapp.local	api-svc
admin.myapp.local	admin-svc

Example test command:

curl http://api.myapp.local
⚙️ Lab Setup
Enable Ingress Controller (Minikube)
minikube addons enable ingress

Verify controller is running:

kubectl get pods -n ingress-nginx

Verify ingress class:

kubectl get ingressclass
Deploy Backend Applications

Apply backend deployments and services:

kubectl apply -f 01-deployments-and-services.yaml

Verify deployments and services:

kubectl get deploy,svc
📄 Ingress Configuration (Single Manifest)

Example Ingress configuration structure:

paths:
  - path: /
    service: webapp-service

  - path: /api
    service: api-service

  - path: /admin
    service: admin-service

hosts:
  - myapp.local
  - api.myapp.local
  - admin.myapp.local

Apply the ingress resource:

kubectl apply -f ingress-lab.yaml

Check ingress status:

kubectl get ingress
🧪 Testing the Ingress

After deployment, retrieve the Ingress Controller IP:

kubectl get ingress

Then test routing using curl.

Path-Based Routing Tests
curl http://<INGRESS-IP>/
curl http://<INGRESS-IP>/api
curl http://<INGRESS-IP>/admin
Host-Based Routing Tests

You can simulate DNS resolution using:

curl --resolve "myapp.local:80:<ADDRESS>" http://myapp.local

curl --resolve "api.myapp.local:80:<ADDRESS>" http://api.myapp.local

curl --resolve "admin.myapp.local:80:<ADDRESS>" http://admin.myapp.local
🖥️ Lab Demonstration

Below is an example output from the running lab environment.

This demonstrates:

Successful routing through the Ingress controller

Correct service responses

Kubernetes traffic flow through NGINX

📁 Project Structure
kubernetes-ingress-lab
│
├── 01-deployments-and-services.yaml
├── 02-ingress-basic-path.yaml
├── 03-ingress-host-based.yaml
├── 04-ingress-default-backend.yaml
├── 05-ingress-path-types.yaml
├── 06-ingress-full-lab.yaml
│
├── ingress-architecture.png
├── lab-demo.png
│
└── README.md
🎯 Key Learning Outcomes

By completing this lab you will understand:

How Ingress replaces multiple NodePort services

Difference between Path-based vs Host-based routing

How NGINX Ingress Controller manages external traffic

Using curl --resolve to simulate DNS locally

Implementing production-style routing in Kubernetes

👨‍💻 Author

Hassan Elhabbal

DevOps Engineer

🔗 GitHub
https://github.com/enghassanelhabbal-DevOps

🔗 LinkedIn
https://linkedin.com/in/hassan-elhabbal

⭐ If you found this project helpful, consider giving the repository a star.


---

If you want, I can also help you **upgrade this README to a top 1% DevOps GitHub README** by adding:

- 🧩 Kubernetes badges  
- 🖼 animated architecture  
- 📊 DevOps tools stack section  
- 🎥 GIF demo (very attractive on GitHub)  
- ⭐ a professional repo header like big DevOps projects.
add all in one part to easy copy

Below is the entire professional README.md in one single block so you can copy it بسهولة مرة واحدة and paste it directly into your GitHub README.md.

Make sure your repo contains these images:

ingress-architecture.png (your infographic)

lab-demo.png (terminal or cluster screenshot)

# 🚀 Kubernetes Ingress Lab

A hands-on **Kubernetes Ingress Lab** demonstrating how to expose multiple services through a **single entry point using the NGINX Ingress Controller**.

This project explains how **Path-Based Routing** and **Host-Based Routing** work in Kubernetes and how an **Ingress resource can replace multiple NodePort services**.

The lab simulates a **production-style traffic routing architecture** where a single Ingress controller manages external traffic and routes requests to multiple backend services.

---

# 📊 Architecture Overview

![Ingress Architecture](./ingress-architecture.png)

The **NGINX Ingress Controller** acts as a **reverse proxy and load balancer**, receiving requests from external clients and routing them to the correct Kubernetes services.

### Components

| Component | Description |
|-----------|-------------|
| Client | Sends HTTP requests using curl |
| Ingress Controller | NGINX reverse proxy handling routing rules |
| Services | Kubernetes ClusterIP services |
| Pods | Backend containers running applications |

---

# 🔀 Routing Types

## 1️⃣ Path-Based Routing

Routes requests depending on the **URL path**.

Example routing:

| Path | Service |
|-----|--------|
| `/` | webapp-service |
| `/api` | api-service |
| `/admin` | admin-service |

Example test command:

```bash
curl http://<INGRESS-IP>/api
2️⃣ Host-Based Routing

Routes traffic using hostnames or subdomains.

Example routing:

Host	Service
myapp.local	webapp-svc
api.myapp.local	api-svc
admin.myapp.local	admin-svc

Example command:

curl http://api.myapp.local
⚙️ Lab Setup
Enable Ingress Controller (Minikube)
minikube addons enable ingress

Verify controller is running:

kubectl get pods -n ingress-nginx

Verify ingress class:

kubectl get ingressclass
📦 Deploy Backend Applications

Deploy the backend services and applications.

kubectl apply -f 01-deployments-and-services.yaml

Verify deployment status:

kubectl get deploy,svc
📄 Ingress Configuration

Example of a single manifest ingress configuration.

paths:
  - path: /
    service: webapp-service

  - path: /api
    service: api-service

  - path: /admin
    service: admin-service

hosts:
  - myapp.local
  - api.myapp.local
  - admin.myapp.local

Apply ingress:

kubectl apply -f ingress-lab.yaml

Check ingress address:

kubectl get ingress
🧪 Testing the Ingress

After retrieving the Ingress Controller IP, test routing.

Path-Based Routing Test
curl http://<INGRESS-IP>/
curl http://<INGRESS-IP>/api
curl http://<INGRESS-IP>/admin
Host-Based Routing Test

Use curl with --resolve to simulate DNS locally.

curl --resolve "myapp.local:80:<ADDRESS>" http://myapp.local

curl --resolve "api.myapp.local:80:<ADDRESS>" http://api.myapp.local

curl --resolve "admin.myapp.local:80:<ADDRESS>" http://admin.myapp.local
🖥️ Lab Demonstration

This screenshot demonstrates:

Traffic entering the Ingress controller

Correct routing to backend services

Successful responses from Kubernetes pods

📁 Repository Structure
kubernetes-ingress-lab
│
├── 01-deployments-and-services.yaml
├── 02-ingress-basic-path.yaml
├── 03-ingress-host-based.yaml
├── 04-ingress-default-backend.yaml
├── 05-ingress-path-types.yaml
├── 06-ingress-full-lab.yaml
│
├── ingress-architecture.png
├── lab-demo.png
│
└── README.md
🎯 Key Learning Outcomes

By completing this lab you will understand:

How Ingress replaces multiple NodePort services

Difference between Path-Based Routing vs Host-Based Routing

How NGINX Ingress Controller handles external traffic

Using curl --resolve to simulate DNS

Implementing production-style routing in Kubernetes

👨‍💻 Author

Hassan Elhabbal

DevOps Engineer

GitHub
https://github.com/enghassanelhabbal-DevOps

LinkedIn
https://linkedin.com/in/hassan-elhabbal
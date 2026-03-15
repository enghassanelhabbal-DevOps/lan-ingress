Kubernetes Ingress Lab

A practical lab demonstrating how Kubernetes Ingress works using the NGINX Ingress Controller with both Path-based Routing and Host-based Routing.

This lab simulates how real production environments expose multiple services through a single entry point.

Architecture Overview

The Ingress Controller acts as a reverse proxy and load balancer, routing external traffic to the correct backend services.

Components
Component	Description
Client	Sends HTTP request using curl
Ingress Controller	NGINX reverse proxy managing routing rules
Services	Kubernetes services exposing backend applications
Pods	Application containers running inside the cluster
Routing Methods
1️⃣ Path-Based Routing

Routes requests based on URL path.

Example:

Path	Service
/	webapp-service
/api	api-service
/admin	admin-service

Example command:

curl http://<INGRESS-IP>/api
2️⃣ Host-Based Routing

Routes traffic based on domain name.

Example:

Host	Service
myapp.local	webapp-svc
api.myapp.local	api-svc
admin.myapp.local	admin-svc

Example command:

curl http://api.myapp.local
Lab Setup
Enable Ingress Controller (Minikube)
minikube addons enable ingress

Verify controller:

kubectl get pods -n ingress-nginx
Deploy Backend Applications
kubectl apply -f 01-deployments-and-services.yaml

Check resources:

kubectl get deploy,svc
Ingress Manifest (Single File Architecture)

Example structure:

paths:
  - /       -> webapp-service
  - /api    -> api-service
  - /admin  -> admin-service

hosts:
  - myapp.local
  - api.myapp.local
  - admin.myapp.local

Apply ingress:

kubectl apply -f ingress-lab.yaml
Testing Commands

Test routing using curl.

Path Routing
curl http://<INGRESS-IP>/api
curl http://<INGRESS-IP>/admin
Host Routing
curl http://api.myapp.local
curl http://admin.myapp.local
Lab Demonstration

Here is an additional screenshot from the lab execution:

This demonstrates:

Service routing

Ingress rule matching

Response from backend pods

Key Learning Points

How Ingress replaces multiple NodePort services

Difference between Path vs Host routing

Using NGINX Ingress Controller

Testing Ingress without modifying DNS using:

curl --resolve

Example:

curl --resolve "myapp.local:80:<ADDRESS>" http://myapp.local/api
Repository Structure
kubernetes-ingress-lab
│
├── 01-deployments-and-services.yaml
├── ingress-lab.yaml
├── ingress-architecture.png
├── lab-demo.png
└── README.md
Author

Hassan Elhabbal

DevOps Engineer

GitHub
https://github.com/enghassanelhabbal-DevOps

LinkedIn
https://linkedin.com/in/hassan-elhabbal

If you want, I can also help you make the README more professional and viral on GitHub by adding:

🚀 badges (Kubernetes / DevOps / CKA)

📊 architecture diagram section

📹 demo GIF

⭐ GitHub project style used by top DevOps repos.
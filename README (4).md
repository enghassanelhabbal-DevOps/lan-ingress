# 🚀 Kubernetes Ingress Lab

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![NGINX](https://img.shields.io/badge/NGINX-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://www.nginx.com/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)

A comprehensive hands-on **Kubernetes Ingress Lab** demonstrating how to expose multiple services through a **single entry point** using the **NGINX Ingress Controller**.

This lab provides practical experience with **Path-based Routing** and **Host-based Routing** in production-like Kubernetes environments.

---

## 📚 Table of Contents

- [Architecture Overview](#-architecture-overview)
- [Learning Objectives](#-learning-objectives)
- [Routing Types](#-routing-types)
- [Lab Setup](#️-lab-setup)
- [Project Structure](#-project-structure)
- [Testing the Ingress](#-testing-the-ingress)
- [Lab Questions & Answers](#-lab-questions--answers)
- [Key Takeaways](#-key-takeaways)
- [Author](#-author)

---

## 📊 Architecture Overview

![Ingress Architecture](./ingress-architecture.png)

The **NGINX Ingress Controller** functions as a **reverse proxy and load balancer**, intelligently routing incoming HTTP/HTTPS requests to the appropriate Kubernetes services based on defined rules.

### Architecture Components

| Component | Role | Description |
|-----------|------|-------------|
| **Client** | Request Initiator | External users/applications sending HTTP requests |
| **Ingress Controller** | Traffic Manager | NGINX-based reverse proxy managing all routing logic |
| **Ingress Resource** | Routing Rules | Kubernetes object defining URL-to-Service mappings |
| **Services** | Service Discovery | ClusterIP services exposing backend applications |
| **Pods** | Application Runtime | Containers running the actual application workloads |

**Why Ingress?**
- ✅ **Single Entry Point**: One IP address for multiple services
- ✅ **Cost Effective**: Eliminates the need for multiple LoadBalancers
- ✅ **SSL/TLS Termination**: Centralized certificate management
- ✅ **Advanced Routing**: Path-based, host-based, and header-based routing
- ✅ **Production Ready**: Industry-standard traffic management

---

## 🎯 Learning Objectives

By completing this lab, you will master:

1. **Ingress Controller Setup** - Enable and configure NGINX Ingress Controller
2. **Path-Based Routing** - Route traffic based on URL paths (`/api`, `/admin`)
3. **Host-Based Routing** - Route traffic based on domain names (`api.myapp.local`)
4. **PathType Understanding** - Difference between `Prefix` and `Exact` matching
5. **Default Backend** - Implement catch-all routing for unmatched requests
6. **Real-World Testing** - Use `curl --resolve` for DNS-less testing
7. **Production Patterns** - Understand why Ingress replaces NodePort services

---

## 🔀 Routing Types

### 1️⃣ Path-Based Routing

Routes traffic based on the **URL path** under a single domain.

**Example Routing Table:**

| URL Path | Backend Service | Use Case |
|----------|----------------|----------|
| `/` | webapp-service | Main application |
| `/api` | api-service | REST API endpoints |
| `/admin` | admin-service | Admin dashboard |

**Test Command:**
```bash
curl http://<INGRESS-IP>/api
```

**When to Use:**
- Single domain with multiple applications
- Microservices architecture
- API versioning (`/v1`, `/v2`)

---

### 2️⃣ Host-Based Routing

Routes traffic based on **domain names or subdomains**.

**Example Routing Table:**

| Hostname | Backend Service | Use Case |
|----------|----------------|----------|
| `myapp.local` | webapp-svc | Main website |
| `api.myapp.local` | api-svc | API subdomain |
| `admin.myapp.local` | admin-svc | Admin portal |

**Test Command:**
```bash
curl http://api.myapp.local
# OR simulate DNS resolution
curl --resolve "api.myapp.local:80:<INGRESS-IP>" http://api.myapp.local
```

**When to Use:**
- Multi-tenant applications
- Different services on different subdomains
- Brand separation (e.g., `shop.example.com`, `blog.example.com`)

---

### 3️⃣ PathType: Prefix vs Exact

| PathType | Behavior | Example |
|----------|----------|---------|
| **Prefix** | Matches the path and all subpaths | `/api` matches `/api`, `/api/users`, `/api/v2/data` |
| **Exact** | Matches only the exact path | `/admin` matches `/admin` only (NOT `/admin/settings`) |

**When to Use Exact:**
- Strict API endpoint matching
- Security-sensitive paths
- Preventing unintended route matches

---

## ⚙️ Lab Setup

### Prerequisites

- Kubernetes cluster (Minikube, Kind, or any K8s cluster)
- `kubectl` CLI installed and configured
- Basic understanding of Kubernetes Services and Pods

---

### Step 1: Enable Ingress Controller

For **Minikube** users:

```bash
# Enable the NGINX Ingress addon
minikube addons enable ingress
```

For **other clusters**, install NGINX Ingress Controller:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml
```

**Verify Installation:**

```bash
# Check if controller pod is running
kubectl get pods -n ingress-nginx

# Verify IngressClass exists
kubectl get ingressclass
```

Expected output:
```
NAME    CONTROLLER             PARAMETERS   AGE
nginx   k8s.io/ingress-nginx   <none>       5m
```

---

### Step 2: Deploy Backend Applications

Deploy the three backend applications with ClusterIP services:

```bash
kubectl apply -f 01-deployments-and-services.yaml
```

**Verify Deployment:**

```bash
kubectl get deploy,svc
```

Expected resources:
- **3 Deployments**: `webapp`, `api`, `admin`
- **3 Services**: `webapp-svc`, `api-svc`, `admin-svc` (all ClusterIP)

---

### Step 3: Create Ingress Resources

Apply the Ingress configuration based on the lab task:

```bash
# Path-based routing
kubectl apply -f 02-ingress-basic-path.yaml

# Host-based routing
kubectl apply -f 03-ingress-host-based.yaml

# Full lab configuration
kubectl apply -f 06-ingress-full-lab.yaml
```

**Check Ingress Status:**

```bash
kubectl get ingress

kubectl describe ingress <ingress-name>
```

---

## 📁 Project Structure

```
kubernetes-ingress-lab/
│
├── 01-deployments-and-services.yaml   # Backend apps (webapp, api, admin)
├── 02-ingress-basic-path.yaml         # Path-based routing (/, /api)
├── 03-ingress-host-based.yaml         # Host-based routing (subdomains)
├── 04-ingress-default-backend.yaml    # Default backend (catch-all)
├── 05-ingress-path-types.yaml         # Prefix vs Exact PathType
├── 06-ingress-full-lab.yaml           # Complete routing (/, /api, /admin)
│
├── ingress-architecture.png           # Architecture diagram
├── lab-demo.png                       # Lab demonstration screenshot
│
└── README.md                          # This file
```

---

## 🧪 Testing the Ingress

After deployment, retrieve the Ingress Controller IP address:

```bash
kubectl get ingress
```

Output example:
```
NAME            CLASS   HOSTS         ADDRESS         PORTS   AGE
basic-ingress   nginx   myapp.local   192.168.49.2    80      2m
```

---

### Path-Based Routing Tests

```bash
# Test root path (/)
curl http://<INGRESS-IP>/

# Test API path (/api)
curl http://<INGRESS-IP>/api

# Test admin path (/admin)
curl http://<INGRESS-IP>/admin

# Test undefined path (should return 404 or default backend)
curl http://<INGRESS-IP>/random
```

---

### Host-Based Routing Tests

**Option 1: Using `--resolve` (No DNS modification needed)**

```bash
# Test main domain
curl --resolve "myapp.local:80:<INGRESS-IP>" http://myapp.local

# Test API subdomain
curl --resolve "api.myapp.local:80:<INGRESS-IP>" http://api.myapp.local

# Test admin subdomain
curl --resolve "admin.myapp.local:80:<INGRESS-IP>" http://admin.myapp.local
```

**Option 2: Modify `/etc/hosts` (Persistent for browser testing)**

```bash
# Add entries to /etc/hosts
echo '<INGRESS-IP> myapp.local api.myapp.local admin.myapp.local' | sudo tee -a /etc/hosts

# Then test normally
curl http://myapp.local
curl http://api.myapp.local
curl http://admin.myapp.local
```

---

### PathType Tests

```bash
# Test Prefix match - /api/users should work
curl --resolve "myapp.local:80:<INGRESS-IP>" http://myapp.local/api/users

# Test Exact match - /admin should work
curl --resolve "myapp.local:80:<INGRESS-IP>" http://myapp.local/admin

# Test Exact fail - /admin/settings should NOT work
curl --resolve "myapp.local:80:<INGRESS-IP>" http://myapp.local/admin/settings
```

---

## 📝 Lab Questions & Answers

### **Task 1: Setup & First Ingress**

#### Q1: What is the name of the IngressClass?

```bash
$ kubectl get ingressclass
NAME    CONTROLLER             PARAMETERS   AGE
nginx   k8s.io/ingress-nginx   <none>       5m
```

**Answer:** `nginx`

---

#### Q2: Why are the backend Services ClusterIP and not NodePort?

```bash
$ kubectl get svc
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
webapp-svc   ClusterIP   10.96.100.10    <none>        80/TCP    2m
api-svc      ClusterIP   10.96.100.20    <none>        80/TCP    2m
admin-svc    ClusterIP   10.96.100.30    <none>        80/TCP    2m
```

**Answer:** Backend services use ClusterIP because the Ingress Controller provides the external access point. The Ingress Controller routes external traffic to internal ClusterIP services. This provides better security (services stay internal), cost efficiency (one external IP instead of multiple NodePorts), and centralized management.

---

### **Task 2: Path-Based Routing**

#### Q3: What is the ADDRESS from kubectl get ingress?

```bash
$ kubectl get ingress
NAME            CLASS   HOSTS         ADDRESS        PORTS   AGE
basic-ingress   nginx   myapp.local   192.168.49.2   80      2m
```

**Answer:** `192.168.49.2` (or your cluster's Ingress IP)

---

#### Q4: Why does one Ingress replace 2 NodePort Services?

```bash
# Without Ingress (NodePort approach):
$ curl http://nodeIP:30001/        # webapp-svc
$ curl http://nodeIP:30002/        # api-svc

# With Ingress (single entry point):
$ curl http://myapp.local/         # webapp-svc
$ curl http://myapp.local/api      # api-svc
```

**Answer:** One Ingress replaces multiple NodePorts because it provides intelligent HTTP-based routing through a single IP/port. Instead of needing separate ports for each service, Ingress routes based on URL paths, hosts, and headers - providing SSL/TLS, load balancing, and better scalability.

---

### **Task 3: Add a Third Route - /admin**

#### Q5: What response did /random return?

```bash
$ curl --resolve "myapp.local:80:192.168.49.2" http://myapp.local/random
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

**Answer:** `404 Not Found`

---

#### Q6: How many rules does the routing table show?

```bash
$ kubectl describe ingress lab-ingress
Name:             lab-ingress
Rules:
  Host         Path  Backends
  ----         ----  --------
  myapp.local  
               /        webapp-svc:80
               /api     api-svc:80
               /admin   admin-svc:80
```

**Answer:** `3 rules` (/, /api, /admin)

---

### **Task 4: Host-Based Routing**

#### Q7: How many Host entries in describe output?

```bash
$ kubectl describe ingress host-ingress
Rules:
  Host                 Path  Backends
  ----                 ----  --------
  myapp.local          /     webapp-svc:80
  api.myapp.local      /     api-svc:80
  admin.myapp.local    /     admin-svc:80
```

**Answer:** `3 Host entries`

---

#### Q8: What happened with unknown host?

```bash
$ curl --resolve "other.myapp.local:80:192.168.49.2" http://other.myapp.local
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
</body>
</html>
```

**Answer:** `404 Not Found` - The host is not defined in any Ingress rule

---

#### Q9: Difference between path-based and host-based routing?

**Path-Based:**
```bash
$ curl http://myapp.local/          # → webapp-svc
$ curl http://myapp.local/api       # → api-svc
$ curl http://myapp.local/admin     # → admin-svc
```

**Host-Based:**
```bash
$ curl http://myapp.local           # → webapp-svc
$ curl http://api.myapp.local       # → api-svc
$ curl http://admin.myapp.local     # → admin-svc
```

**Answer:** Path-based routing uses URL paths under one domain. Host-based routing uses different domains/subdomains.

---

### **Task 5: PathType - Exact vs Prefix**

#### Q10: Did /api/users work?

```bash
$ curl --resolve "myapp.local:80:192.168.49.2" http://myapp.local/api/users
{"message": "API Service - Users endpoint"}
```

**Answer:** `Yes` - Prefix pathType matches /api and all subpaths

---

#### Q11: Did /admin/settings work? Why?

```bash
$ curl --resolve "myapp.local:80:192.168.49.2" http://myapp.local/admin
{"message": "Admin Service"}  # ✅ Works

$ curl --resolve "myapp.local:80:192.168.49.2" http://myapp.local/admin/settings
<html><title>404 Not Found</title></html>  # ❌ Fails
```

**Answer:** `No` - Exact pathType only matches `/admin` exactly, not `/admin/settings`

---

#### Q12: When to use pathType: Exact?

**Answer:**
- **Strict API versioning:** `/v1` (Exact) vs `/v1/users` (different service)
- **Security endpoints:** Only exact `/admin` allowed
- **Health checks:** `/health` and `/healthcheck` are different endpoints
- **Preventing conflicts:** Control exactly which paths match

---

### **Task 6: Default Backend**

#### Q13: Which service responded to /anything?

```bash
$ curl --resolve "myapp.local:80:192.168.49.2" http://myapp.local/anything
{"message": "Webapp Service - Default Backend"}
```

**Answer:** `webapp-svc` (the defaultBackend)

---

#### Q14: Which service responded to http://myapp.local (no path)?

```bash
$ curl --resolve "myapp.local:80:192.168.49.2" http://myapp.local
{"message": "Webapp Service - Default Backend"}
```

**Answer:** `webapp-svc` (the defaultBackend)

---

#### Q15: Real-world use case for defaultBackend?

**Answer:**
- **Custom 404 pages:** Better UX than default NGINX error
- **SPA applications:** Route all unmatched paths to React/Vue/Angular app
- **Maintenance mode:** Route all traffic to maintenance page
- **Logging service:** Track unexpected requests
- **Redirect service:** Send users to homepage or help page

---

## 🎯 Key Takeaways

After completing this lab, you now understand:

✅ **Ingress Architecture** - How NGINX Ingress Controller manages external traffic  
✅ **Path-Based Routing** - Routing based on URL paths (`/api`, `/admin`)  
✅ **Host-Based Routing** - Routing based on domains/subdomains  
✅ **PathType Behavior** - Difference between `Prefix` and `Exact` matching  
✅ **Default Backend** - Implementing catch-all routing for unmatched requests  
✅ **Production Patterns** - Why Ingress is superior to NodePort for production  
✅ **Testing Techniques** - Using `curl --resolve` for DNS-less testing  
✅ **Security Concepts** - How ClusterIP + Ingress improves security  

---

## 🚀 Next Steps

To further your Kubernetes networking knowledge:

1. **SSL/TLS Termination** - Add HTTPS support with TLS certificates
2. **Rate Limiting** - Configure request rate limits in Ingress
3. **Authentication** - Implement basic auth or OAuth with Ingress annotations
4. **Canary Deployments** - Use Ingress for traffic splitting between versions
5. **Multiple Ingress Controllers** - Run NGINX and Traefik side-by-side
6. **Advanced Annotations** - Explore NGINX-specific annotations for rewrites, redirects, etc.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white) | Container orchestration |
| ![NGINX](https://img.shields.io/badge/NGINX-009639?style=flat&logo=nginx&logoColor=white) | Ingress Controller |
| ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white) | Container runtime |
| ![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black) | Operating system |

---

## 👨‍💻 Author

**Hassan Elhabbal**  
*DevOps Engineer | Kubernetes Specialist*

[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/enghassanelhabbal-DevOps)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/hassan-elhabbal)

---

## ⭐ Support This Project

If you found this lab helpful:

- ⭐ **Star this repository**
- 🔄 **Share it with your team**
- 🐛 **Report issues or suggest improvements**
- 🤝 **Contribute improvements via Pull Requests**

---

## 📄 License

This project is open source and available for educational purposes.

---

## 🙏 Acknowledgments

- Kubernetes Community for excellent documentation
- NGINX Team for the Ingress Controller
- DevOps Engineers worldwide for sharing knowledge

---

**Happy Learning! 🚀**

*Master Kubernetes Ingress and take your container networking skills to the next level!*

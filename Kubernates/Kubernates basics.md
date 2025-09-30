# ☸️ Kubernetes – Day 1 Notes

---

## 🔹 1. Introduction to Kubernetes & Concepts
- **Kubernetes (k8s)** is an **open-source container orchestration system**.  
- Developed by **Google**, now maintained by **CNCF (Cloud Native Computing Foundation)**.  
- Automates:
  - Deployment
  - Scaling
  - Load balancing
  - Self-healing of containers  

### Key Concepts
- **Cluster** → A set of nodes (machines) managed by Kubernetes.  
- **Node** → Worker machine (VM or physical).  
- **Pod** → Smallest deployable unit in k8s (wraps one or more containers).  
- **Service** → Stable networking endpoint for pods.  
- **Deployment** → Manages desired state of pods.  
- **Namespace** → Logical separation inside a cluster.  

---

## 🔹 2. History of Kubernetes
- Born inside **Google** (used internally as Borg & Omega).  
- Open-sourced in **2014**, donated to CNCF.  
- Inspired by Google’s experience managing **billions of containers per week**.  
- Now the **de facto standard** for container orchestration.  

---

## 🔹 3. Why Learn Kubernetes?
- **Industry Standard** → Used by startups to FAANG.  
- **Cloud-Native Apps** → Helps deploy scalable microservices.  
- **Portability** → Runs anywhere (on-prem, AWS, GCP, Azure).  
- **DevOps Essential** → Works with CI/CD pipelines.  
- **Job Opportunities** → High demand for Kubernetes & cloud skills.  

---

## 🔹 4. Monolithic vs Microservices

### Monolithic
- One big application.
- Hard to scale (must scale the entire app).
- A single failure may crash the whole system.
- Example: Old e-commerce built as **one WAR file** in Java.

### Microservices
- Application broken into **smaller, independent services**.
- Easier scaling (scale only the service in demand).
- Fault isolation → one service crash doesn’t kill the whole system.
- Example: Amazon → cart, payment, recommendations all separate services.

👉 Kubernetes makes **microservices architecture easier to deploy & manage**.

---

## 🔹 5. Kubernetes Architecture

### Components
- **Master Node (Control Plane)**:
  - `kube-apiserver` → Handles API requests.
  - `etcd` → Key-value store for cluster state.
  - `kube-scheduler` → Decides which pod runs where.
  - `kube-controller-manager` → Ensures desired state.

- **Worker Node**:
  - `kubelet` → Agent on each node, manages pods.
  - `kube-proxy` → Handles networking/routing.
  - Container runtime (Docker, containerd).

---

### kubectl
- Command-line tool to interact with k8s cluster.

#### Examples:
```bash
# Check cluster info
kubectl cluster-info

# List nodes
kubectl get nodes

# Run nginx pod
kubectl run nginx --image=nginx

# List pods
kubectl get pods

# Describe a pod
kubectl describe pod nginx


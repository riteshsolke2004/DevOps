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

##☸️ Why We Use Kubernetes?

Kubernetes (k8s) is used because managing containers manually becomes chaotic when you have many services running across multiple machines.

✅ Main Reasons We Use Kubernetes:

Container Orchestration

Automates deployment, scheduling, scaling, and management of containers.

Example: Instead of running docker run for 50 services manually, k8s handles it for you.

Scalability

Scale up or down based on traffic.

Example: Add more pods automatically during Black Friday sales.

Self-Healing

If a pod crashes, Kubernetes restarts it automatically.

Keeps the system stable without manual intervention.

Load Balancing & Service Discovery

Provides built-in load balancing.

Services in k8s have stable DNS names, even if pod IPs change.

Rolling Updates & Rollbacks

Deploy new versions of apps without downtime.

Rollback easily if something goes wrong.

Portability

Runs anywhere → on-premises, AWS, GCP, Azure, DigitalOcean, or even local.

Resource Optimization

Schedules pods to best-fit available CPU, RAM, etc.

Ensures efficient use of infrastructure.

Microservices Friendly

Best suited for applications built as microservices.

Each microservice runs in its own pod but communicates easily with others.

📌 Simple Example

Imagine you’re running an e-commerce app with services:

frontend, backend, database, payment, cart.

Without Kubernetes → you’d run each container manually, manage failures, and scale them yourself.
With Kubernetes →

One YAML file defines everything.

K8s deploys them, scales them, balances traffic, and heals failures.

👉 In short:
We use Kubernetes because it makes running containers in production safe, reliable, and scalable — without burning ourselves out managing everything manually.

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


🔹 6. AWS Setup for Hands-On

Kubernetes can be deployed on AWS EKS (Elastic Kubernetes Service).

EKS handles control plane; you manage worker nodes.

Setup steps (high-level):

Install AWS CLI & eksctl.

Create an EKS cluster:

eksctl create cluster --name my-cluster --region us-east-1


Configure kubectl to use this cluster:

aws eks update-kubeconfig --region us-east-1 --name my-cluster


Verify:

kubectl get nodes

🔹 7. What is Kind Cluster?

Kind (Kubernetes in Docker) = tool for running Kubernetes locally using Docker.

Good for learning, testing, and CI/CD.

Lightweight alternative to Minikube.

Creates clusters inside Docker containers.

🔹 8. Kind Setup on Local
Install Kind
# Linux/macOS
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
chmod +x ./kind
mv ./kind /usr/local/bin/kind

# Windows (PowerShell)
choco install kind

🔹 9. Kind Cluster Creation
✅ Create a Cluster
kind create cluster --name demo-cluster

✅ List Clusters
kind get clusters

✅ Use kubectl with Kind
kubectl cluster-info --context kind-demo-cluster
kubectl get nodes

✅ Delete a Cluster
kind delete cluster --name demo-cluster

📌 Quick Summary

Kubernetes = container orchestration platform.

Born at Google → CNCF project.

Solves scaling, load balancing, self-healing.

Monolithic ❌ vs Microservices ✅ → Kubernetes makes microservices manageable.

Architecture: Control Plane + Worker Nodes.

Tools: kubectl for commands, kind for local clusters.

Cloud: AWS EKS for production-ready managed clusters.

Local: Kind for dev & testing clusters.

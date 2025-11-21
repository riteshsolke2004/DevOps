🚀 Kubernetes — Complete Lecture Notes & Cheat Sheet

Ready-to-use README for learning Kubernetes. Covers core concepts, architecture, YAML examples, kubectl commands, storage, networking, scaling, security, observability, and best practices.

📚 Table of Contents

Introduction / Overview

Architecture

Core Concepts

Pods

Namespaces

Deployments / ReplicaSets

StatefulSets / DaemonSets / Jobs

Labels & Selectors

Storage

PV / PVC / StorageClass

Networking

Services

Ingress

Configuration & Secrets

Health Checks (Probes)

Scaling & Autoscaling

Scheduling & Node Placement

Security & RBAC

Observability / Monitoring

Example Projects / Ideas

Commands Cheat Sheet

YAML Examples

Best Practices

Summary / Key Takeaways

🧭 Introduction / Overview

Kubernetes (K8s) is an open-source system to automate deployment, scaling, and management of containerized applications.

You declare your desired state using YAML

Kubernetes continuously reconciles actual → desired state

Handles failures, autoscaling, networking, storage, and rollouts

These notes convert lecture content into a practical, developer-friendly reference.

🏗 Architecture
Control Plane

kube-apiserver — central API endpoint

etcd — distributed key-value store for cluster state

kube-scheduler — assigns Pods to nodes

kube-controller-manager — runs controllers (replicas, endpoints, nodes)

Worker Nodes

kubelet — manages Pods on the node

container runtime — Docker/containerd

kube-proxy — networking rules for Services

🔑 Core Concepts
🌱 Pods

Smallest deployable unit

Can contain 1+ containers

Share network + storage

Ephemeral → use Deployments for production

🗂 Namespaces

Logical isolation inside cluster

Separate dev/stage/prod

Apply RBAC & quotas per namespace

📦 Deployments / ReplicaSets

Deployment handles:

Rolling updates

Rollbacks

Scaling

ReplicaSet ensures fixed # of Pods

Deployments manage ReplicaSets behind the scenes

🧱 StatefulSets / DaemonSets / Jobs
Resource	Purpose
StatefulSet	Databases, apps needing stable identity/storage
DaemonSet	One Pod on every node (logs, monitoring agents)
Job	Run-to-completion tasks
CronJob	Scheduled Jobs
🏷 Labels & Selectors

Attach labels like app: frontend
Used by:

Services

Deployments

HPA

NetworkPolicies

💾 Storage
PV / PVC / StorageClass

PV — actual storage resource

PVC — user request for storage

StorageClass — defines type (fast/slow, SSD/HDD)

Workflow:

Define StorageClass (optional)

User creates PVC

PV is bound or dynamically created

Mount PVC into Pod

🌐 Networking
Services

Stable access to Pods.

Type	Use
ClusterIP	internal traffic
NodePort	exposes port externally
LoadBalancer	cloud LBs
ExternalName	DNS alias
Ingress

HTTP routing

Host/path-based routing

Requires Ingress Controller (NGINX / Traefik)

⚙ Configuration & Secrets
ConfigMaps

Store non-sensitive config.

Secrets

Store sensitive data (passwords, TLS certs).
Base64 encoded in YAML.

❤️ Health Checks (Probes)
Probe	Purpose
Liveness	restart if unhealthy
Readiness	remove from Service endpoints
Startup	allow slow boot apps

Probes:

httpGet

tcpSocket

exec

📈 Scaling & Autoscaling
Manual scaling:
kubectl scale deployment myapp --replicas=5

Horizontal Pod Autoscaler (HPA)

Scales based on CPU/memory/custom metrics.

Vertical Pod Autoscaler (VPA)

Adjusts resource requests/limits.

Cluster Autoscaler

Adds/removes nodes automatically (cloud).

🧭 Scheduling & Node Placement

Node labels + nodeAffinity

Taints & tolerations

Pod anti-affinity to spread replicas

🔐 Security & RBAC

ServiceAccounts

Roles / RoleBindings

ClusterRoles / ClusterRoleBindings

Least privilege

NetworkPolicies

Image scanning

📊 Observability / Monitoring

Logs: kubectl logs

Metrics: Prometheus

Dashboards: Grafana

Tracing: Jaeger / Zipkin

Alerts: Alertmanager

🌟 Example Projects / Ideas

3-tier app (Nginx → API → PostgreSQL StatefulSet)

Chat app with Redis

CI/CD pipeline with GitHub Actions → k8s deploy

EKS demo with autoscaling + monitoring

🧩 Commands Cheat Sheet
Apply configs
kubectl apply -f <file.yaml>

View resources
kubectl get pods
kubectl get deployments
kubectl get svc
kubectl get pv,pvc
kubectl get nodes

Logs & shell
kubectl logs <pod>
kubectl exec -it <pod> -- sh

Scale
kubectl scale deployment myapp --replicas=5

Update image
kubectl set image deployment/myapp app=myimage:v2

Delete
kubectl delete -f <file.yaml>
kubectl delete pod <pod>

Namespace
kubectl create namespace dev
kubectl get all -n dev

🧾 YAML Examples
Pod
apiVersion: v1
kind: Pod
metadata:
  name: demo-pod
  labels:
    app: demo
spec:
  containers:
    - name: nginx
      image: nginx:1.23
      ports:
        - containerPort: 80

Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
        - name: web
          image: nginx:1.23
          ports:
            - containerPort: 80

Service
apiVersion: v1
kind: Service
metadata:
  name: demo-svc
spec:
  selector:
    app: demo
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP

Ingress
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-ingress
spec:
  rules:
    - host: demo.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: demo-svc
                port:
                  number: 80

PV & PVC
apiVersion: v1
kind: PersistentVolume
metadata:
  name: demo-pv
spec:
  capacity:
    storage: 1Gi
  accessModes: [ReadWriteOnce]
  hostPath:
    path: /mnt/data/demo
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: demo-pvc
spec:
  accessModes: [ReadWriteOnce]
  resources:
    requests:
      storage: 1Gi

ConfigMap & Secret
apiVersion: v1
kind: ConfigMap
metadata:
  name: demo-config
data:
  APP_ENV: "dev"
  LOG_LEVEL: "info"
---
apiVersion: v1
kind: Secret
metadata:
  name: demo-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQ=

HPA
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: demo-hpa
spec:
  scaleTargetRef:
    kind: Deployment
    name: demo-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50

🧠 Best Practices

Don't run bare Pods — use Deployments/StatefulSets

Always define resource requests & limits

Use readiness + liveness probes

Store config in ConfigMaps / Secrets, not images

Use RBAC + Namespaces for security

Set up full observability stack

Use rolling updates with easy rollbacks

Scan images and use signed images

🔥 Summary / Key Takeaways

Kubernetes abstracts containers into Pods and higher controllers

YAML + kubectl = main workflow

Services + Ingress handle traffic

PV/PVC handle persistent storage

Autoscaling and probes → resilient apps

Security + Monitoring = production-ready clusters

Kubernetes — Complete Lecture Notes & Cheat Sheet

Ready-to-use README for a learning repo. Covers core concepts, architecture, YAML examples, kubectl commands, storage, networking, scaling, security, examples & best practices.

Table of contents

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

YAML Examples (copy-ready)

Best Practices

Summary / Key Takeaways

Introduction / Overview

Kubernetes (K8s) is the open-source system to orchestrate containerized apps: deploy, scale, self-heal, and expose services. You declare desired state in YAML -> K8s controller loops reconcile actual state to match it. This README turns lecture concepts into a practical quick-reference.

Architecture

K8s has two logical planes:

Control plane (master)

kube-apiserver — central API (all requests go here).

etcd — distributed key-value store (cluster state).

kube-scheduler — which node runs which Pods.

kube-controller-manager — controllers that reconcile resources.

Worker nodes

kubelet — agent that runs Pods.

container runtime — containerd / dockerd, etc.

kube-proxy — implements Service networking rules.

Core Concepts
Pods

Smallest deployable unit. One or more containers sharing network and storage.

Ephemeral: don't use bare Pods for production; use higher-level controllers.

Namespaces

Virtual clusters within a cluster. Use for dev, staging, prod, or teams.

Isolate resources, apply quota & RBAC per namespace.

Deployments & ReplicaSets

Deployment manages stateless Pods + rolling updates + rollback.

ReplicaSet ensures N replicas of a Pod template are running (usually managed by Deployment).

StatefulSet / DaemonSet / Jobs

StatefulSet — stateful apps needing stable network IDs & storage (databases).

DaemonSet — run one Pod on each (or selected) node (logging, node agents).

Job — run-to-completion workload; CronJob — scheduled Job.

Labels & Selectors

Attach key: value labels to objects.

Use selectors in Services, Deployments to choose Pods.

Storage
PV / PVC / StorageClass

PersistentVolume (PV) — cluster-level resource representing physical storage.

PersistentVolumeClaim (PVC) — user request for storage.

StorageClass — dynamic provisioning rules (fast/slow, gp2, etc).

Pattern:

Create StorageClass (optional for dynamic provisioning).

Create PVC (claims a PV or requests dynamic PV).

Mount PVC into Pod spec under volumes.

Networking
Services

Provide stable access to ephemeral Pods.

Types:

ClusterIP (default) — internal-only.

NodePort — expose service on each node externally.

LoadBalancer — provision cloud LB.

ExternalName — alias to external DNS.

Service → selects Pods by label selector and load-balances.

Ingress

Routes HTTP(S) traffic to Services based on host/path rules.

Requires an Ingress Controller (NGINX, Traefik, etc).

Configuration & Secrets

ConfigMap — store non-sensitive config (env vars, config files).

Secret — store sensitive data (passwords, TLS keys). Can be mounted as files or env vars. Base64 encoded in YAML.

Health Checks (Probes)

Liveness probe — if fails, kubelet restarts container.

Readiness probe — if fails, container removed from Service endpoints (no traffic).

Startup probe — allow longer start-up period for heavy apps.

Examples use httpGet, tcpSocket, or exec.

Scaling & Autoscaling

Manual scale: kubectl scale deployment/my-deploy --replicas=5.

Horizontal Pod Autoscaler (HPA): scales replicas based on CPU/memory/custom metrics.

Vertical Pod Autoscaler (VPA): adjusts resource requests/limits.

Cluster Autoscaler: adds/removes nodes based on scheduling needs (cloud provider).

Scheduling & Node Placement

Node labels & nodeAffinity — schedule Pods to specific nodes.

Taints & tolerations — repel Pods from certain nodes unless they tolerate the taint.

Pod anti-affinity — spread Pods across nodes.

Security & RBAC

Use ServiceAccounts for Pods to access API.

RBAC: Roles / ClusterRoles + RoleBindings / ClusterRoleBindings for fine-grained permissions.

Recommendations: least privilege, network policies, scan images, imagePullPolicy set properly.

Observability / Monitoring

Logs: kubectl logs (or centralized via Fluentd/Logstash).

Metrics: Prometheus + Grafana for CPU/memory/Custom metrics.

Tracing: Jaeger/Zipkin.

Alerts: Prometheus Alertmanager.

Example Projects / Ideas

3-tier web app: nginx frontend → backend service → PostgreSQL StatefulSet.

Chat app (frontend + websocket backend + Redis).

CI/CD demo: GitHub Actions → build image → push to registry → apply kubectl manifests.

Production EKS demo: set HPA & Cluster Autoscaler + monitoring + IAM for service accounts.

Commands Cheat Sheet

Common commands to keep handy:

# apply manifests
kubectl apply -f <file.yaml>

# view resources
kubectl get pods
kubectl get pods -n <namespace>
kubectl get deployments
kubectl get svc
kubectl get pv,pvc
kubectl get nodes

# describe & inspect
kubectl describe pod <pod-name>
kubectl describe deployment <deployment-name>

# logs & exec
kubectl logs <pod-name>            # logs for first container
kubectl logs <pod-name> -c <c>     # logs for container c
kubectl exec -it <pod> -- /bin/sh

# scale
kubectl scale deployment myapp --replicas=5

# rolling update (change image)
kubectl set image deployment/my-deploy my-container=myimage:tag

# delete
kubectl delete -f <file.yaml>
kubectl delete pod <pod-name>

# namespaces
kubectl create namespace dev
kubectl get all -n dev

# resource usage
kubectl top nodes
kubectl top pods

# port-forward (for local dev)
kubectl port-forward svc/my-service 8080:80

# HPA
kubectl autoscale deployment my-deploy --cpu-percent=50 --min=2 --max=10
kubectl get hpa

YAML Examples (copy-ready)
Pod (simple)
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
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"
            limits:
              cpu: "500m"
              memory: "256Mi"
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10

Service (ClusterIP)
apiVersion: v1
kind: Service
metadata:
  name: demo-svc
spec:
  selector:
    app: demo
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP

Ingress (basic)
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

PersistentVolume & PersistentVolumeClaim (hostPath example for demo)
apiVersion: v1
kind: PersistentVolume
metadata:
  name: demo-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data/demo

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: demo-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi


Mount PVC into a Pod:

spec:
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: demo-pvc
  containers:
    - name: app
      image: myapp:latest
      volumeMounts:
        - mountPath: /data
          name: data

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
  DB_PASSWORD: cGFzc3dvcmQ=   # base64 of 'password'


Use as env in Pod:

env:
  - name: APP_ENV
    valueFrom:
      configMapKeyRef:
        name: demo-config
        key: APP_ENV
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: demo-secret
        key: DB_PASSWORD

HPA (Horizontal Pod Autoscaler)
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: demo-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
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

RBAC (Role + RoleBinding example)
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: demo
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "watch", "list"]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: demo
subjects:
  - kind: User
    name: alice
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io

Best Practices

Use Deployments / StatefulSets instead of a single Pod.

Set resource requests & limits for every container.

Use readiness and liveness probes.

Keep Config and Secrets out of images; inject them at runtime.

Use Namespaces and RBAC to enforce least privilege.

Prefer rolling updates and keep rollbacks ready.

Implement observability (metrics + logs + tracing).

Keep cluster nodes small enough to scale but large enough to reduce scheduling overhead.

Use image scanning and signed images where possible.

Summary / Key Takeaways

Kubernetes abstracts containers into Pods and provides robust patterns for stateless and stateful workloads.

YAML manifests + kubectl = the core workflow.

Services & Ingress handle networking; PV/PVC handle persistent storage.

Autoscaling & probes make apps resilient and responsive.

Security, monitoring, and proper scheduling are essential for production readiness.

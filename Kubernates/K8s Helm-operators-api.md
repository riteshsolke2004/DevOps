# Kubernetes, Helm, Operators & Kubernetes API — README

> A concise but thorough cheat‑sheet and guide covering Kubernetes (kubectl), Helm (charts), Operators (CRDs & controllers), and interacting with the Kubernetes API. Includes commands, examples, and best practices.

---

## Table of contents

1. [Quick intro](#quick-intro)
2. [Prerequisites](#prerequisites)
3. [kubectl — essential commands & usage](#kubectl---essential-commands--usage)
4. [YAML basics & common resource examples](#yaml-basics--common-resource-examples)
5. [Helm — Charts & commands](#helm---charts--commands)
6. [Operators — CRD, SDK, lifecycle](#operators---crd-sdk-lifecycle)
7. [Kubernetes API — endpoints & how to talk to it](#kubernetes-api---endpoints--how-to-talk-to-it)
8. [Debugging & troubleshooting](#debugging--troubleshooting)
9. [Security & RBAC snippets](#security--rbac-snippets)
10. [Helpful tips & best practices](#helpful-tips--best-practices)
11. [References & further reading](#references--further-reading)

---

## Quick intro

Kubernetes (k8s) is a container orchestration platform that manages workloads and services. `kubectl` is the CLI to interact with clusters. Helm is the package manager for Kubernetes (charts). Operators are controllers that automate application-specific tasks using CustomResourceDefinitions (CRDs). The Kubernetes API is a RESTful interface that underpins every operation in the cluster.

---

## Prerequisites

* `kubectl` installed and configured (`kubectl version`, `kubectl config current-context`).
* Access to a cluster (minikube, kind, k3s, GKE, EKS, AKS, etc.).
* `helm` (v3+) installed.
* (Operators) `operator-sdk` or `kubebuilder` if you plan to develop operators.
* `docker` (or another container runtime) for building images.

---

## kubectl — essential commands & usage

### Cluster info

```bash
kubectl version --client && kubectl cluster-info
kubectl config get-contexts
kubectl config current-context
```

### Work with namespaces

```bash
kubectl get ns
kubectl create namespace my-ns
kubectl config set-context --current --namespace=my-ns
kubectl delete namespace my-ns
```

### Common resource commands

```bash
kubectl get pods                # list pods
kubectl get pods -A             # list pods across namespaces
kubectl get deployments
kubectl get svc
kubectl get nodes
kubectl describe pod <pod-name>
kubectl logs <pod-name> [-c container]
kubectl exec -it <pod-name> -- /bin/sh
kubectl apply -f file.yaml      # create/update resources
kubectl delete -f file.yaml
kubectl delete pod <pod-name>
```

### Apply vs Create vs Replace

* `kubectl apply -f`: declarative; track changes with server-side apply.
* `kubectl create -f`: imperative create (fails if exists).
* `kubectl replace -f`: replace existing resource.

### Port-forwarding & proxy

```bash
kubectl port-forward svc/my-service 8080:80
kubectl proxy --port=8001           # access API locally at http://127.0.0.1:8001
```

### Exposing workloads

```bash
kubectl expose deployment my-app --type=LoadBalancer --port=80 --target-port=8080
```

### Rollouts & scaling

```bash
kubectl scale deployment my-app --replicas=5
kubectl rollout status deployment/my-app
kubectl rollout history deployment/my-app
kubectl rollout undo deployment/my-app
```

### Resource editing & patch

```bash
kubectl edit deployment my-app   # edit resource in $EDITOR
kubectl patch deployment my-app --type='json' -p='[{"op":"replace

```

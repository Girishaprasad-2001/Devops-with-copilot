# Kubernetes A to Z Commands Cheat Sheet

This is a comprehensive Kubernetes (`kubectl`) command reference from beginner to advanced.

---

# A. Cluster Information

## Display Cluster Information

```bash
kubectl cluster-info
```

## Show Client and Server Versions

```bash
kubectl version
```

## List All API Resources

```bash
kubectl api-resources
```

## List All Supported API Versions

```bash
kubectl api-versions
```

---

# B. Context & Configuration

## View Current Context

```bash
kubectl config current-context
```

## List Contexts

```bash
kubectl config get-contexts
```

## Switch Context

```bash
kubectl config use-context <context-name>
```

## View Config

```bash
kubectl config view
```

## Set Default Namespace

```bash
kubectl config set-context --current --namespace=dev
```

---

# C. Namespace Commands

## List Namespaces

```bash
kubectl get namespaces
```

or

```bash
kubectl get ns
```

## Create Namespace

```bash
kubectl create namespace dev
```

## Delete Namespace

```bash
kubectl delete namespace dev
```

---

# D. Pod Commands

## List Pods

```bash
kubectl get pods
```

or

```bash
kubectl get po
```

## Detailed Output

```bash
kubectl get pods -o wide
```

## Show Pod Details

```bash
kubectl describe pod nginx
```

## Create Pod

```bash
kubectl run nginx --image=nginx
```

## Delete Pod

```bash
kubectl delete pod nginx
```

## Execute Command Inside Pod

```bash
kubectl exec -it nginx -- bash
```

## View Logs

```bash
kubectl logs nginx
```

## Follow Logs

```bash
kubectl logs -f nginx
```

## Copy File

```bash
kubectl cp file.txt nginx:/tmp/
```

---

# E. Deployment Commands

## List Deployments

```bash
kubectl get deployments
```

## Create Deployment

```bash
kubectl create deployment nginx --image=nginx
```

## Scale Deployment

```bash
kubectl scale deployment nginx --replicas=5
```

## Edit Deployment

```bash
kubectl edit deployment nginx
```

## Rollout Status

```bash
kubectl rollout status deployment/nginx
```

## Rollout History

```bash
kubectl rollout history deployment/nginx
```

## Rollback

```bash
kubectl rollout undo deployment/nginx
```

---

# F. ReplicaSet Commands

```bash
kubectl get rs
```

```bash
kubectl describe rs nginx
```

```bash
kubectl delete rs nginx
```

---

# G. StatefulSet Commands

```bash
kubectl get statefulsets
```

```bash
kubectl get sts
```

```bash
kubectl describe sts mysql
```

---

# H. DaemonSet Commands

```bash
kubectl get daemonsets
```

```bash
kubectl get ds
```

```bash
kubectl describe ds fluentd
```

---

# I. Service Commands

## List Services

```bash
kubectl get svc
```

## Create ClusterIP Service

```bash
kubectl expose deployment nginx --port=80
```

## Create NodePort Service

```bash
kubectl expose deployment nginx \
--type=NodePort \
--port=80
```

## Describe Service

```bash
kubectl describe svc nginx
```

---

# J. ConfigMap Commands

## Create ConfigMap

```bash
kubectl create configmap app-config \
--from-literal=env=prod
```

## View ConfigMaps

```bash
kubectl get cm
```

## Describe ConfigMap

```bash
kubectl describe cm app-config
```

---

# K. Secret Commands

## Create Secret

```bash
kubectl create secret generic db-secret \
--from-literal=password=Password123
```

## List Secrets

```bash
kubectl get secrets
```

## Describe Secret

```bash
kubectl describe secret db-secret
```

---

# L. Node Commands

## List Nodes

```bash
kubectl get nodes
```

## Node Details

```bash
kubectl describe node worker01
```

## Cordon Node

```bash
kubectl cordon worker01
```

## Drain Node

```bash
kubectl drain worker01 --ignore-daemonsets
```

## Uncordon Node

```bash
kubectl uncordon worker01
```

---

# M. Persistent Volume Commands

## List PVs

```bash
kubectl get pv
```

## List PVCs

```bash
kubectl get pvc
```

## Describe PV

```bash
kubectl describe pv pv01
```

## Describe PVC

```bash
kubectl describe pvc claim01
```

---

# N

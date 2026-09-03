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

# N. Job Commands

## Create Job

```bash
kubectl create job test-job \
--image=busybox
```

## Get Jobs

```bash
kubectl get jobs
```

## Delete Job

```bash
kubectl delete job test-job
```

---

# O. CronJob Commands

## List CronJobs

```bash
kubectl get cronjobs
```

## Create CronJob

```bash
kubectl create cronjob backup \
--schedule="*/5 * * * *" \
--image=busybox
```

---

# P. Apply & Manifest Commands

## Apply YAML

```bash
kubectl apply -f deployment.yaml
```

## Create Resources

```bash
kubectl create -f deployment.yaml
```

## Delete Resources

```bash
kubectl delete -f deployment.yaml
```

## Dry Run

```bash
kubectl apply -f deployment.yaml --dry-run=client
```

---

# Q. Resource Monitoring

## Pod Resource Usage

```bash
kubectl top pods
```

## Node Resource Usage

```bash
kubectl top nodes
```

---

# R. Events

## Cluster Events

```bash
kubectl get events
```

## Sort Events

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

# S. Label Commands

## Add Label

```bash
kubectl label pod nginx app=web
```

## Remove Label

```bash
kubectl label pod n*inx app-
```

## Filter by Label

*``bash
kubectl get pods -l app=web*```

---

# T. Taints & Toleration*

## Add Taint

```bash
kubectl ta*nt nodes worker01 key=value:NoSche*ule
```

## Remove Taint

```bash
*ubectl taint nodes worker01 key=va*ue:NoSchedule-
```

---

# U. Anno*ate Resources

## Add Annotation

*``bash
kubectl annotate pod nginx *wner=devops
```

## Remove Annotat*on

```bash
kubectl annotate pod n*inx owner-
```

---

# V. Debugging Commands

## Describe Resource

```bash
kubectl describe pod nginx
```

## Container Logs

```bash
kubectl logs nginx
```

## Previous Container Logs

```bash
kubectl logs nginx --previous
```

## Debug Container

```bash
kubectl debug -it nginx --image=busybox
```

---

# W. Networking Commands

## Port Forward

```bash
kubectl port-forward pod/nginx 8080:80
```

## DNS Check

```bash
kubectl exec -it nginx -- nslookup kubernetes.default
```

---

# X. Advanced JSON/YAML Output

## YAML Output

```bash
kubectl get pod nginx -o yaml
```

## JSON Output

```bash
kubectl get pod nginx -o json
```

## Custom Columns

```bash
kubectl get pods \
-o custom-columns=NAME:.metadata.name,STATUS:.status.phase
```

---

# Y. Useful Shortcuts

## All Resources

```bash
kubectl get all
```

## Watch Resources

```bash
kubectl get pods -w
```

## Explain Resource

```bash
kubectl explain deployment
```

## Explain Field

```bash
kubectl explain deployment.spec
```

---

# Z. Must-Know Production Commands

## Check Everything

```bash
kubectl get all -A
```

## Find Error Pods

```bash
kubectl get pods -A | grep Error
```

## Pods on Specific Node

```bash
kubectl get pods -o wide
```

## Pod Logs Across Namespaces

```bash
kubectl logs -n production nginx
```

## Force Delete Pod

```bash
kubectl delete pod nginx --grace-period=0 --force
```

## Check Certificate Expiry

```bash
kubeadm certs check-expiration
```

## ETCD Snapshot Backup

```bash
ETCDCTL_API=3 etcdctl snapshot save backup.db
```

---

# Top 20 Commands Every DevOps Engineer Uses Daily

```bash
kubectl get pods -A
kubectl get nodes
kubectl get svc
kubectl get deployments
kubectl describe pod <pod>
kubectl logs <pod>
kubectl logs -f <pod>
kubectl exec -it <pod> -- bash
kubectl top pods
kubectl top nodes
kubectl apply -f file.yaml
kubectl delete -f file.yaml
kubectl rollout status deployment/<name>
kubectl rollout undo deployment/<name>
kubectl scale deployment <name> --replicas=3
kubectl get events
kubectl get all -A
kubectl port-forward svc/<svc> 8080:80
kubectl cordon <node>
kubectl drain <node> --ignore-daemonsets
```

---

# Summary

This cheat sheet covers the most commonly used Kubernetes commands for:

- CKA (Certified Kubernetes Administrator)
- CKAD (Certified Kubernetes Application Developer)
- CKS (Certified Kubernetes Security Specialist)
- DevOps Engineers
- Site Reliability Engineers (SRE)
- Production Support Engineers
- Platform Engineers
- Cloud Engineers
- Kubernetes Administrators

⭐ Keep this README as a quick reference guide for daily Kubernetes operations and troubleshooting.

# ELK Stack + Filebeat + Metricbeat Setup & Workflow for Linux Servers and Kubernetes

A complete guide for setting up and understanding:

- Elasticsearch
- Logstash
- Kibana
- Filebeat
- Metricbeat
- Linux Server Monitoring
- Kubernetes Log Collection
- Kubernetes Metrics Collection

This guide is useful for:

- DevOps Engineers
- SRE Engineers
- Observability Engineers
- Platform Engineers
- Production Support Teams

---

# ELK Stack Overview

ELK stands for:

```text
E = Elasticsearch
L = Logstash
K = Kibana
```

Modern deployments often use:

```text
Elastic Stack
├── Elasticsearch
├── Kibana
├── Filebeat
├── Metricbeat
├── Logstash
└── Fleet
```

---

# Architecture Overview

## Linux Server Monitoring

```text
Linux Server
     |
     |
     +----------------------+
     |                      |
     ▼                      ▼
 Filebeat              Metricbeat
     |                      |
     |                      |
     +----------+-----------+
                |
                ▼
            Logstash
                |
                ▼
         Elasticsearch
                |
                ▼
             Kibana
```

---

# Kubernetes Monitoring Architecture

```text
Pods
 |
 ▼
Container Logs
 |
 ▼
Filebeat DaemonSet
 |
 ▼
Logstash
 |
 ▼
Elasticsearch
 |
 ▼
Kibana


Kubernetes Nodes
 |
 ▼
Metricbeat DaemonSet
 |
 ▼
Elasticsearch
 |
 ▼
Kibana
```

---

# Component Responsibilities

---

## Elasticsearch

Stores:

- Logs
- Metrics
- Events
- Application Data

Default Port:

```text
9200
```

---

## Logstash

Responsible for:

- Data Processing
- Log Parsing
- Transformation
- Filtering

Default Port:

```text
5044
```

---

## Kibana

Responsible for:

- Dashboards
- Visualization
- Log Search
- Monitoring

Default Port:

```text
5601
```

---

## Filebeat

Collects:

- System Logs
- Application Logs
- Container Logs
- Kubernetes Pod Logs

Default Output:

```text
Elasticsearch
or
Logstash
```

---

## Metricbeat

Collects:

- CPU Metrics
- Memory Metrics
- Disk Metrics
- System Metrics
- Kubernetes Metrics

---

# Important Ports

```text
9200 Elasticsearch
9300 Elasticsearch Cluster
5044 Logstash Beats Input
5601 Kibana
```

---

# Step 1: Install Elasticsearch

## Add Repository

Ubuntu Example

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
```

---

## Install

```bash
apt update
```

```bash
apt install elasticsearch -y
```

---

## Configure

```bash
vi /etc/elasticsearch/elasticsearch.yml
```

```yaml
cluster.name: prod-cluster

node.name: elastic-node1

network.host: 0.0.0.0

http.port: 9200
```

---

## Start Service

```bash
systemctl daemon-reload

systemctl enable elasticsearch

systemctl start elasticsearch
```

---

## Verify

```bash
curl http://localhost:9200
```

Expected:

```json
{
"name":"elastic-node1"
}
```

---

# Step 2: Install Kibana

## Install

```bash
apt install kibana -y
```

---

## Configure

```bash
vi /etc/kibana/kibana.yml
```

```yaml
server.port: 5601

server.host: "0.0.0.0"

elasticsearch.hosts:
  - "http://localhost:9200"
```

---

## Start Service

```bash
systemctl enable kibana

systemctl start kibana
```

---

## Access

```text
http://SERVER-IP:5601
```

---

# Step 3: Install Logstash

## Install

```bash
apt install logstash -y
```

---

## Logstash Pipeline

```bash
vi /etc/logstash/conf.d/beats.conf
```

```ruby
input {

  beats {
    port => 5044
  }

}

filter {

}

output {

  elasticsearch {
    hosts => ["localhost:9200"]
  }

}
```

---

## Start Logstash

```bash
systemctl enable logstash

systemctl start logstash
```

---

## Verify

```bash
ss -tulnp | grep 5044
```

---

# Step 4: Install Filebeat on Linux Server

## Install

```bash
apt install filebeat -y
```

---

## Configure Filebeat

```bash
vi /etc/filebeat/filebeat.yml
```

Example:

```yaml
filebeat.inputs:

- type: log

  enabled: true

  paths:
    - /var/log/*.log

output.logstash:

  hosts:
 *  - "10.0.0.10:5044"
```

---

## *tart Service

```bash
systemctl en*ble filebeat

systemctl start file*eat
```

---

## Verify

```bash
s*stemctl status filebeat
```

---

* Filebeat Workflow

```text
Linux *ogs
      |
      ▼
Filebeat
     *|
      ▼
Logstash
      |
      ▼*Elasticsearch
      |
      ▼
Kiba*a Dashboard
```

---

# Step 5: In*tall Metricbeat

## Install

```ba*h
apt install metricbeat -y
```

-*-

## Configure

```bash
vi /etc/m*tricbeat/metricbeat.yml
```

```ya*l
metricbeat.modules:

- module: s*stem

  metricsets:
    - cpu
    * memory
    - network
    - diskio*    - filesystem

output.elasticse*rch:

  hosts:
    - "http://10.0.*.10:9200"
```

---

## Enable Modu*es

```bash
metricbeat modules ena*le system
```

---

## Start

```b*sh
systemctl enable metricbeat

sy*temctl start metricbeat
```

---

* Metricbeat Workflow

```text
CPU
*emory
Disk
Network
    |
    ▼
Met*icbeat
    |
    ▼
Elasticsearch
 *  |
    ▼
Kibana
```

---

# Linux*Server Monitoring Flow

```text
Li*ux Server
     |
     +-----------*+
     |            |
     ▼      *     ▼
Filebeat    Metricbeat
    *|            |
     ▼            ▼*     Elasticsearch
           |
  *        ▼
         Kibana
```

---*
# Kubernetes Setup

---

# Filebe*t DaemonSet

## Deploy Filebeat

`*`bash
kubectl apply -f filebeat-ku*ernetes.yaml
```

---

## Filebeat*DaemonSet Architecture

```text
Wo*ker Node 1
      |
      └── Fileb*at Pod

Worker Node 2
      |
    * └── Filebeat Pod

Worker Node 3
 *    |
      └── Filebeat Pod
```

*--

## Collects

```text
/var/log/*ontainers/
/var/log/pods/
```

---*
# Kubernetes Filebeat Workflow

`*`text
Pod Logs
    |
    ▼
Filebea* DaemonSet
    |
    ▼
Logstash
  * |
    ▼
Elasticsearch
    |
    ▼*Kibana
```

---

# Metricbeat Daem*nSet

Deploy:

```bash
kubectl apply -f metricbeat-kubernetes.yaml
```

---

## Collects

- Node Metrics
- Pod Metrics
- Container Metrics
- Kubelet Metrics

---

# Kubernetes Metricbeat Flow

```text
Pods
Nodes
Containers
      |
      ▼
Metricbeat
      |
      ▼
Elasticsearch
      |
      ▼
Kibana
```

---

# Useful Elasticsearch Commands

## Cluster Health

```bash
curl localhost:9200/_cluster/health?pretty
```

## Nodes

```bash
curl localhost:9200/_cat/nodes?v
```

## Indices

```bash
curl localhost:9200/_cat/indices?v
```

---

# Useful Kibana Steps

```text
Management
   |
   ▼
Stack Management
   |
   ▼
Index Patterns
```

Create:

```text
filebeat-*
metricbeat-*
```

---

# Verify Beats

## Filebeat

```bash
filebeat test config
```

```bash
filebeat test output
```

---

## Metricbeat

```bash
metricbeat test config
```

```bash
metricbeat test output
```

---

# Troubleshooting Commands

## Elasticsearch

```bash
systemctl status elasticsearch

journalctl -u elasticsearch -f
```

---

## Kibana

```bash
systemctl status kibana

journalctl -u kibana -f
```

---

## Logstash

```bash
systemctl status logstash

journalctl -u logstash -f
```

---

## Filebeat

```bash
systemctl status filebeat

journalctl -u filebeat -f
```

---

## Metricbeat

```bash
systemctl status metricbeat

journalctl -u metricbeat -f
```

---

# Port Verification

```bash
ss -tulnp | grep 9200

ss -tulnp | grep 5601

ss -tulnp | grep 5044
```

---

# End-to-End Production Workflow

```text
Linux Server / Kubernetes Cluster
                |
                |
        +-------+-------+
        |               |
        ▼               ▼
    Filebeat      Metricbeat
        |               |
        +-------+-------+
                |
                ▼
            Logstash
                |
                ▼
         Elasticsearch
                |
                ▼
             Kibana
                |
                ▼
     Search • Dashboards • Alerts
```

---

# Top Daily Commands

```bash
systemctl status elasticsearch
systemctl status kibana
systemctl status logstash
systemctl status filebeat
systemctl status metricbeat

curl localhost:9200

curl localhost:9200/_cat/indices?v

curl localhost:9200/_cat/nodes?v

filebeat test config
metricbeat test config

kubectl get pods -A

kubectl logs <filebeat-pod>

kubectl logs <metricbeat-pod>

journalctl -u elasticsearch -f
journalctl -u kibana -f
journalctl -u logstash -f
journalctl -u filebeat -f
journalctl -u metricbeat -f
```

---

# Summary

### Filebeat

```text
Collects Logs
```

### Metricbeat

```text
Collects Metrics
```

### Logstash

```text
Parses & Transforms Data
```

### Elasticsearch

```text
Stores Logs & Metrics
```

### Kibana

```text
Visualizes Data Through Dashboards
```

### End-to-End Flow

```text
Linux / Kubernetes
        ↓
Filebeat + Metricbeat
        ↓
Logstash
        ↓
Elasticsearch
        ↓
Kibana
        ↓
Dashboards, Search, Alerts
```

⭐ This ELK + Beats setup is one of the most commonly used observability platforms for enterprise Linux servers, Kubernetes clusters, cloud infrastructure, DevOps monitoring, and production support.


# Linux Server Configuration + Kubernetes Deployment YAML + DevOps Workflow, use the following format:

# Linux Server Configuration and Kubernetes Deployment Guide

A practical guide for configuring Linux servers, deploying applications on Kubernetes, and managing production workloads.

---

# Prerequisites

- Linux Server (Ubuntu/RHEL/CentOS/Amazon Linux)
- Docker Installed
- Kubernetes Cluster
- kubectl Configured
- Git Installed
- Internet Connectivity

---

# 1. Linux Server Configuration

## Update Server Packages

### Ubuntu/Debian

```bash
sudo apt update && sudo apt upgrade -y
```

### RHEL/CentOS

```bash
sudo yum update -y
```

---

## Install Git

### Ubuntu

```bash
sudo apt install git -y
```

### RHEL/CentOS

```bash
sudo yum install git -y
```

Verify installation:

```bash
git --version
```

---

## Install Docker

### Ubuntu

```bash
sudo apt update
sudo apt install docker.io -y

sudo systemctl enable docker
sudo systemctl start docker
```

Verify:

```bash
docker --version
docker info
```

Add current user to docker group:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

## Install Kubectl

```bash
curl -LO https://dl.k8s.io/release/stable/bin/linux/amd64/kubectl

chmod +x kubectl

sudo mv kubectl /usr/local/bin/
```

Verify:

```bash
kubectl version --client
```

---

# 2. Kubernetes Cluster Verification

Check cluster status:

```bash
kubectl cluster-info
```

Check nodes:

```bash
kubectl get nodes
```

Expected Output:

```text
NAME       STATUS   ROLES
master01   Ready    control-plane
worker01   Ready    worker
worker02   Ready    worker
```

---

# 3. Create Namespace

```bash
kubectl create namespace dev
```

Verify:

```bash
kubectl get ns
```

---

# 4. Kubernetes Deployment YAML

Create file:

```bash
vi deployment.yaml
```

## deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment
  namespace: dev

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest

        ports:
        - containerPort: 80

        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"

          limits:
            cpu: "500m"
            memory: "512Mi"
```

---

# 5. Create Service YAML

## service.yaml

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service
  namespace: dev

spec:
  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80

  type: NodePort
```

---

# 6. Deploy Application

Apply deployment:

```bash
kubectl apply -f deployment.yaml
```

Apply service:

```bash
kubectl apply -f service.yaml
```

Verify resources:

```bash
kubectl get all -n dev
```

---

# 7. Scaling Deployment

Scale to 5 replicas:

```bash
kubectl scale deployment nginx-deployment \
--replicas=5 \
-n dev
```

Verify:

```bash
kubectl get pods -n dev
```

---

# 8. Rolling Update

Update Image:

```bash
kubectl set image deployment/nginx-deployment \
nginx=nginx:1.27 \
-n dev
```

Check rollout:

```bash
kubectl rollout status deployment/nginx-deployment \
-n dev
```

---

# 9. Rollback Deployment

View history:

```bash
kubectl rollout history deployment/nginx-deployment
```

Rollback:

```bash
kubectl rollout undo deployment/nginx-deployment \
-n dev
```

---

# 10. Pod Troubleshooting

View pods:

```bash
kubectl get pods -n dev
```

Describe pod:

```bash
kubectl describe pod <pod-name> -n dev
```

View logs:

```bash
kubectl logs <pod-name> -n dev
```

Open shell:

```bash
kubectl exec -it <pod-name> -n dev -- bash
```

---

# 11. Resource Monitoring

Node utilization:

```bash
kubectl top nodes
```

Pod utilization:

```bash
kubectl top pods -n dev
```

---

# 12. Delete Resources

Delete deployment:

```bash
kubectl delete deployment nginx-deployment -n dev
```

Delete service:

```bash
kubectl delete service nginx-service -n dev
```

Delete namespace:

```bash
kubectl delete namespace dev
```

---

# Production DevOps Workflow

```text
Developer
    ↓
Git Push
    ↓
GitHub/GitLab
    ↓
Jenkins / Azure DevOps
    ↓
Docker Build
    ↓
Docker Push
    ↓
Kubernetes Deployment
    ↓
Service Exposure
    ↓
Monitoring (Prometheus/Grafana)
```

---

# Common Kubernetes Commands

```bash
kubectl get pods 
```

# DevSecOps & GitOps A to Z Guide
## Commands, Workflow, Configuration & Step-by-Step Setup

A complete DevSecOps and GitOps reference covering:

- DevSecOps Fundamentals
- GitOps Fundamentals
- Security Best Practices
- CI/CD Security
- Infrastructure as Code (IaC)
- Kubernetes Security
- Container Security
- Secret Management
- SAST / DAST / SCA
- GitHub Actions
- Jenkins
- ArgoCD
- FluxCD
- Trivy
- SonarQube
- HashiCorp Vault
- Kubernetes Deployment
- Production Workflow

---

# What is DevSecOps?

DevSecOps =

```text
Development
      +
Security
      +
Operations
```

Security is integrated into every phase of SDLC.

---

# DevSecOps Lifecycle

```text
Plan
  |
  ▼
Code
  |
  ▼
Build
  |
  ▼
Test
  |
  ▼
Security Scan
  |
  ▼
Deploy
  |
  ▼
Monitor
```

---

# What is GitOps?

GitOps is a deployment methodology where Git becomes the:

```text
Single Source Of Truth
```

Changes in Git automatically update infrastructure and applications.

---

# GitOps Workflow

```text
Developer
     |
     ▼
Git Repository
     |
     ▼
GitOps Tool
(ArgoCD / Flux)
     |
     ▼
Kubernetes Cluster
     |
     ▼
Application Deployment
```

---

# DevSecOps Architecture

```text
Developer
    |
    ▼
GitHub / GitLab
    |
    ▼
Jenkins / GitHub Actions
    |
    ▼
SAST Scan
    |
    ▼
Dependency Scan
    |
    ▼
Container Scan
    |
    ▼
Build Docker Image
    |
    ▼
Push To Registry
    |
    ▼
GitOps Repository Update
    |
    ▼
ArgoCD
    |
    ▼
Kubernetes Cluster
```

---

# DevSecOps Tools

## Source Control

```text
Git
GitHub
GitLab
Bitbucket
Azure DevOps
```

---

## CI/CD

```text
Jenkins
GitHub Actions
GitLab CI
Azure Pipelines
Tekton
```

---

## Security Tools

```text
SonarQube
Trivy
Snyk
OWASP ZAP
Checkov
Gitleaks
Bandit
```

---

## GitOps Tools

```text
ArgoCD
FluxCD
```

---

## Secret Management

```text
HashiCorp Vault
AWS Secrets Manager
Azure Key Vault
Kubernetes Secrets
```

---

# Step 1: Git Repository Setup

Create Repository:

```bash
git init
```

Add Files:

```bash
git add .
```

Commit:

```bash
git commit -m "Initial Commit"
```

Push:

```bash
git push origin main
```

---

# Git Security Commands

## Verify Commit

```bash
git verify-commit <commit-id>
```

## Sign Commit

```bash
git commit -S -m "secure commit"
```

---

# Step 2: SonarQube Setup

Run Using Docker:

```bash
docker run -d \
--name sonarqube \
-p 9000:9000 \
sonarqube
```

Access:

```text
http://SERVER-IP:9000
```

---

# Sonar Scan Command

```bash
mvn sonar:sonar
```

---

# Step 3: Dependency Security Scan

## OWASP Dependency Check

```bash
mvn org.owasp:dependency-check-maven:check
```

---

# Step 4: Secret Scanning

## Gitleaks

Install:

```bash
gitleaks version
```

Scan:

```bash
gitleaks detect
```

---

# Step 5: Build Docker Image

```bash
docker build -t myapp:v1 .
```

Verify:

```bash
docker images
```

---

# Step 6: Container Security Scan

## Trivy Installation

```bash
trivy --version
```

---

## Scan Docker Image

```bash
trivy image myapp:v1
```

---

## Scan Kubernetes YAML

```bash
trivy config deployment.yaml
```

---

## Scan Filesystem

```bash
trivy fs .
```

---

# Step 7: Push Docker Image

Login:

```bash
docker login
```

Push:

```bash
docker push myrepo/myapp:v1
```

---

# Step 8: Store Secrets in Vault

Login:

```bash
vault login
```

Store Secret:

```bash
vault kv put secret/app \
db_user=admin \
db_pass=password
```

Read:

```bash
vault kv get secret/app
```

---

# Step 9: Kubernetes Deployment

Apply:

```bash
kubectl apply -f deployment.yaml
```

Verify:

```bash
kubectl get pods
```

---

# Secure Kubernetes Commands

Check RBAC:

```bash
kubectl auth can-i '*' '*'
```

View Secrets:

```bash
kubectl get secrets
```

Network Policies:

```bash
kubectl get networkpolicy
```

---

# Kubernetes Security Best Practices

```text
RBAC
Network Policy
Pod Security
Secrets Encryption
Image Scanning
Admission Controllers
```

---

# ArgoCD Installation

Create Namespace:

```bash
kubectl create namespace argocd
```

Install:

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

# Verify ArgoCD

```bash
kubectl get pods -n argocd
```

---

# Access ArgoCD

```bash
kubectl port-forward svc/argocd-server \
-n argocd 8080:443
```

---

# Get Admin Password

```bash
kubectl -n argocd \
get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d
```

---

# Create GitOps Application

```bash
argocd app create myapp \
--repo https://github.com/company/app.git \
--path manifests \
--dest-server https://kubernetes.default.svc \
--dest-namespace default
```

---

# Sync Application

```bash
argocd app sync myapp
```

---

# Check Status

```bash
argocd app get myapp
```

---

# FluxCD Installation

Install Flux CLI:

```bash
flux --version
```

Bootstrap:

```bash
flux bootstrap github \
--owner=myorg \
--repository=gitops \
--branch=main
```

---

# GitOps Flow

```text
Developer Commit
      |
      ▼
Git Repository
      |
      ▼
ArgoCD Watches Repo
      |
      ▼
Detect Change
      |
      ▼
Sync Cluster
      |
      ▼
Application Updated
```

---

# GitHub Actions Pipeline

```yaml
name: CI

on:
  push:

jobs:

  build:

    runs-on: ubuntu-latest

    steps:

    - uses: actions/checkout@v4

    - name: Build

      run: mvn package

    - name: Trivy Scan

      run: trivy fs .

    - name: Docker Build

      run: docker build -t app .
```

---

# Jenkins DevSecOps Pipeline

```groovy
pipeline {

 agent any

 stages {

  stage('Build') {
   steps {
    sh 'mvn clean package'
   }
  }

  stage('SonarQube') {
   steps {
    sh 'mvn sonar:sonar'
   }
  }

  stage('Trivy Scan') {
   steps {
    sh 'trivy image myapp:v1'
   }
  }

  stage('Deploy') {
   steps {
    sh 'kubectl apply -f deployment.yaml'
   }
  }

 }
}
```

---

# Infrastructure as Code Security

## Terraform Scan

```bash
checkov -d .
```

---

## Terraform Validate

```bash
terraform validate
```

---

## Terraform Plan

```bash
terraform plan
```

---

# Kubernetes GitOps Repository Structure

```text
gitops-repo/

├── applications
│
├── environments
│   ├── dev
│   ├── test
│   └── prod
│
├── manifests
│
├── argocd
│
└── helm
```

---

# Security Gates

```text
Source Code
    |
    ▼
SAST

    |
    ▼
Dependency Scan

    |
    ▼
Secret Scan

    |
    ▼
Container Scan

    |
    ▼
IaC Scan

    |
    ▼
Deployment
```

---

# Daily DevSecOps Commands

```bash
git status

git pull

docker build .

docker push

kubectl get pods

kubectl get deployments

kubectl logs

trivy image app

trivy fs .

gitleaks detect

vault kv get secret/app

terraform validate

terraform plan

argocd app list

argocd app sync

argocd app get
```

---

# Troubleshooting Commands

## Kubernetes

```bash
kubectl get pods -A

kubectl describe pod <pod>

kubectl logs <pod>
```

---

## ArgoCD

```bash
argocd app get myapp

argocd app sync myapp
```

---

## Vault

```bash
vault status
```

---

## Docker

```bash
docker ps

docker images
```

---

## Trivy

```bash
trivy image myapp:v1
```

---

# End-to-End Enterprise Workflow

```text
Developer
   |
   ▼

GitHub/GitLab
   |
   ▼

GitHub Actions / Jenkins
   |
   ├── SAST (SonarQube)
   |
   ├── SCA (Dependency Check)
   |
   ├── Secret Scan (Gitleaks)
   |
   ├── Container Scan (Trivy)
   |
   └── IaC Scan (Checkov)
   |
   ▼

Docker Registry
   |
   ▼

GitOps Repository
   |
   ▼

ArgoCD / FluxCD
   |
   ▼

Kubernetes
   |
   ▼

Prometheus + Grafana
   |
   ▼

Production
```

---

# DevSecOps Maturity Model

```text
Level 1 : CI/CD
Level 2 : Automated Security Scans
Level 3 : Shift Left Security
Level 4 : GitOps Deployment
Level 5 : Continuous Compliance
Level 6 : Fully Automated Secure Platform
```

---

# Summary

This guide covers:

- DevSecOps Fundamentals
- GitOps Fundamentals
- Git Security
- SonarQube
- Trivy
- Gitleaks
- OWASP Dependency Check
- HashiCorp Vault
- Jenkins
- GitHub Actions
- Kubernetes Security
- ArgoCD
- FluxCD
- Terraform Security
- CI/CD Security Gates
- Enterprise Deployment Workflow

⭐ Keep this README as a complete DevSecOps and GitOps reference for DevOps Engineers, SREs, Platform Engineers, Cloud Engineers, Security Engineers, and Production Support Teams.

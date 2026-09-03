# Azure A to Z Guide
## Architecture, Configuration, Workflow & Administration

A complete Azure Cloud reference guide covering architecture, configuration, administration, monitoring, security, networking, DevOps, Kubernetes, disaster recovery, and production support.

---

# Table of Contents

1. Azure Virtual Machines (VMs)
2. Azure Virtual Networks (VNet)
3. Network Security Groups (NSG)
4. Azure Load Balancer
5. Azure Application Gateway
6. Azure Kubernetes Service (AKS)
7. Azure Container Registry (ACR)
8. Azure App Services
9. Azure Storage Accounts
10. Azure Key Vault
11. Azure Monitor
12. Log Analytics
13. Azure Active Directory (Azure AD)
14. Azure DNS
15. Azure DevOps
16. Recovery Services Vault
17. Azure Traffic Manager
18. End-to-End Azure Architecture
19. Azure Administration Commands
20. Production Support & Troubleshooting

---

# 1. Azure Virtual Machines (VMs)

## What is Azure VM?

Azure Virtual Machines provide Infrastructure as a Service (IaaS) that allows organizations to run Windows and Linux servers in Azure.

### Common Use Cases

- Application Hosting
- Database Servers
- Development Environments
- SAP Systems
- Enterprise Applications

### Architecture

```text
Azure Subscription
        |
Resource Group
        |
Virtual Machine
        |
Operating System
        |
Application
```

### VM Components

- OS Disk
- Data Disk
- NIC
- Public IP
- NSG
- Virtual Network

### Administration

```bash
az vm list
az vm start
az vm stop
az vm restart
az vm delete
```

---

# 2. Azure Virtual Networks (VNet)

## What is VNet?

A Virtual Network provides secure private communication between Azure resources.

### Components

- Address Space
- Subnets
- Route Tables
- NSG
- VPN Gateway

### Architecture

```text
VNet
 ├── Web Subnet
 ├── App Subnet
 └── DB Subnet
```

### Administration

```bash
az network vnet list

az network vnet create \
--name ProdVnet \
--resource-group RG1
```

---

# 3. Network Security Groups (NSG)

## What is NSG?

NSG controls inbound and outbound traffic using security rules.

### Typical Rules

```text
Allow HTTP 80
Allow HTTPS 443
Allow SSH 22
Deny All
```

### Architecture

```text
Internet
   |
NSG Rules
   |
Subnet
   |
VM
```

### Administration

```bash
az network nsg create

az network nsg rule create
```

---

# 4. Azure Load Balancer

## What is Azure Load Balancer?

Distributes traffic across VMs.

### Types

- Public Load Balancer
- Internal Load Balancer

### Workflow

```text
User Request
      |
Load Balancer
      |
-------------
|           |
VM1       VM2
```

### Use Cases

- High Availability
- Scalability
- Fault Tolerance

---

# 5. Azure Application Gateway

## What is Application Gateway?

Layer-7 Web Traffic Load Balancer.

### Features

- SSL Termination
- URL Routing
- Web Application Firewall (WAF)
- Session Affinity

### Architecture

```text
Internet
     |
Application Gateway
     |
------------------
|                |
Web App1     Web App2
```

### Common Routing

```text
/api/* → Backend API

/admin/* → Admin App

/* → Frontend
```

---

# 6. Azure Kubernetes Service (AKS)

## What is AKS?

Managed Kubernetes service provided by Azure.

### Components

```text
AKS Cluster
      |
Node Pools
      |
Pods
      |
Containers
```

### AKS Workflow

```text
Developer
   |
Docker Image
   |
ACR
   |
AKS Cluster
   |
Pods
```

### Administration

```bash
az aks list

az aks get-credentials

kubectl get nodes

kubectl get pods
```

---

# 7. Azure Container Registry (ACR)

## What is ACR?

Private Docker Registry in Azure.

### Workflow

```text
Developer
     |
Docker Build
     |
Push Image
     |
ACR
     |
AKS Pull
```

### Commands

```bash
az acr create

az acr login

docker push
docker pull
```

---

# 8. Azure App Services

## What is Azure App Service?

Fully managed platform to host:

- Java
- .NET
- Python
- NodeJS
- PHP

### Architecture

```text
Internet
    |
App Service
    |
Application Code
```

### Benefits

- Auto Scaling
- Auto Patching
- Managed Platform

---

# 9. Azure Storage Accounts

## What is Storage Account?

Central storage service.

### Types

- Blob Storage
- File Storage
- Queue Storage
- Table Storage

### Architecture

```text
Storage Account
      |
---------------------
|     |      |      |
Blob Queue Table File
```

### Administration

```bash
az storage account create
```

---

# 10. Azure Key Vault

## What is Key Vault?

Secure storage for:

- Secrets
- Certificates
- Passwords
- Encryption Keys

### Workflow

```text
Application
     |
Managed Identity
     |
Key Vault
```

### Example

```text
Database Password
API Key
TLS Certificate
```

---

# 11. Azure Monitor

## What is Azure Monitor?

Centralized monitoring service.

### Features

- Metrics
- Alerts
- Performance Monitoring
- Dashboards

### Workflow

```text
Azure Resources
        |
Azure Monitor
        |
Alert Rules
        |
Email / Teams
```

---

# 12. Log Analytics

## What is Log Analytics?

Stores and analyzes log data.

### KQL Example

```kusto
Heartbeat
| take 10
```

### Architecture

```text
Azure Resources
      |
Log Analytics Workspace
      |
Kusto Queries
```

---

# 13. Azure Active Directory (Azure AD)

## What is Azure AD?

Identity and Access Management Service.

### Features

- Single Sign-On
- MFA
- Conditional Access
- RBAC

### Architecture

```text
User
   |
Azure AD
   |
Applications
```

### Administration

```bash
az ad user list

az ad group list
```

---

# 14. Azure DNS

## What is Azure DNS?

Hosts and manages DNS zones.

### DNS Records

```text
A Record
AAAA Record
CNAME
TXT
MX
NS
```

### Workflow

```text
Domain
    |
Azure DNS Zone
    |
Azure Application
```

---

# 15. Azure DevOps

## What is Azure DevOps?

Complete DevOps platform.

### Services

- Azure Repos
- Azure Pipelines
- Azure Boards
- Azure Artifacts
- Azure Test Plans

### CI/CD Workflow

```text
Code Commit
      |
Azure Repos
      |
Build Pipeline
      |
Testing
      |
Deployment
      |
Production
```

---

# 16. Recovery Services Vault

## What is Recovery Services Vault?

Provides:

- VM Backup
- Disaster Recovery
- Azure Site Recovery

### Workflow

```text
Production VM
       |
Backup Policy
       |
Recovery Vault
       |
Recovery Point
```

### Benefits

- Business Continuity
- Data Protection
- Disaster Recovery

---

# 17. Azure Traffic Manager

## What is Traffic Manager?

DNS-based Global Load Balancer.

### Routing Methods

- Priority
- Performance
- Geographic
- Weighted

### Architecture

```text
User
 |
Traffic Manager
 |
------------------
|                |
East US      West Europe
```

### Benefits

- Global Distribution
- Disaster Recovery
- Latency Optimization

---

# 18. End-to-End Azure Enterprise Architecture

```text
                    Internet
                         |
                 Traffic Manager
                         |
                 Application Gateway
                         |
                    Azure WAF
                         |
                 Azure Load Balancer
                         |
                -----------------
                |               |
              AKS            App Service
                |               |
                -----------------
                         |
                    Azure SQL
                         |
                   Azure Storage
                         |
                    Key Vault
                         |
                 Azure Monitor
                         |
                  Log Analytics
```

---

# 19. Azure Administration Commands

## Login

```bash
az login
```

## Set Subscription

```bash
az account set --subscription "<subscription>"
```

## Resource Groups

```bash
az group list

az group create \
-n Prod-RG \
-l centralindia
```

## Virtual Machines

```bash
az vm list

az vm start

az vm stop
```

## AKS

```bash
az aks list

kubectl get pods

kubectl get nodes
```

## Storage

```bash
az storage account list
```

---

# 20. Production Support & Troubleshooting

## VM Down

```bash
az vm restart
```

Check:

```bash
Azure Monitor
Boot Diagnostics
Activity Logs
```

---

## AKS Pod Failure

```bash
kubectl get pods

kubectl describe pod

kubectl logs pod-name
```

---

## Network Issue

Check:

```text
NSG Rules

Route Table

DNS Configuration

Load Balancer Health Probe
```

---

## High CPU Usage

```bash
top
htop

kubectl top pod

kubectl top node
```

---

## Storage Issue

Verify:

```text
Storage Keys

Private Endpoint

Firewall Rules

RBAC Permissions
```

---

# Azure Enterprise Workflow

```text
Developer
     |
Azure Repos
     |
Azure Pipeline
     |
Gradle/Maven Build
     |
Docker Build
     |
Azure Container Registry
     |
AKS Deployment
     |
Application Gateway
     |
Traffic Manager
     |
End Users
     |
Azure Monitor
     |
Log Analytics
```

---

# Best Practices

## Security

- Enable MFA
- Use RBAC
- Use Managed Identities
- Store secrets in Key Vault

## Networking

- Use NSGs
- Use Private Endpoints
- Separate VNets/Subnets

## AKS

- Multiple Node Pools
- HPA Autoscaling
- Network Policies

## Monitoring

- Enable Azure Monitor
- Configure Alerts
- Use Log Analytics

## Backup

- Enable Recovery Vault
- Test Restoration Frequently

---

# Summary

This guide covers:

✅ Azure Virtual Machines (VMs)

✅ Virtual Networks (VNet)

✅ Network Security Groups (NSG)

✅ Azure Load Balancer

✅ Azure Application Gateway

✅ Azure Kubernetes Service (AKS)

✅ Azure Container Registry (ACR)

✅ Azure App Services

✅ Azure Storage Accounts

✅ Azure Key Vault

✅ Azure Monitor

✅ Log Analytics

✅ Azure Active Directory (Azure AD)

✅ Azure DNS

✅ Azure DevOps

✅ Recovery Services Vault

✅ Azure Traffic Manager

✅ Azure Architecture

✅ Azure Administration

✅ Production Support & Troubleshooting

⭐ Use this README as a complete Azure Cloud Administrator, Azure DevOps Engineer, Azure Architect, AKS Administrator, and Production Support reference guide.

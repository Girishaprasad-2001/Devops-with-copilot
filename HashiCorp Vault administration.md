# HashiCorp Vault Administration A to Z Commands, Setup & Workflow Guide

A complete HashiCorp Vault reference covering:

- Vault Architecture
- Installation
- Initialization
- Unseal Process
- Authentication Methods
- Secrets Engines
- Policies
- Tokens
- KV Secrets
- Dynamic Secrets
- Kubernetes Integration
- DevOps CI/CD Integration
- Administration Commands
- Monitoring
- Troubleshooting
- Production Best Practices

---

# What is HashiCorp Vault?

HashiCorp Vault is a secrets management and data protection platform used to securely manage:

- Passwords
- API Keys
- Database Credentials
- SSH Keys
- TLS Certificates
- Cloud Credentials
- Kubernetes Secrets

---

# Vault Architecture

```text
Application
      |
      ▼
 Authentication
      |
      ▼
      Vault
      |
 ┌────┼─────┐
 ▼    ▼     ▼

KV   DB   PKI

      |
      ▼

Secrets Returned
```

---

# End-to-End Vault Workflow

```text
Developer/Application
         |
         ▼
Authenticate
         |
         ▼
Vault Token
         |
         ▼
Request Secret
         |
         ▼
Vault Policy Check
         |
         ▼
Vault Secret Engine
         |
         ▼
Secret Delivered
```

---

# Vault Components

## Vault Server

Stores and manages secrets.

Default Port:

```text
8200
```

---

## Storage Backend

Stores Vault Data:

- Raft
- Consul
- DynamoDB
- PostgreSQL

---

## Authentication Methods

Examples:

```text
Token
LDAP
GitHub
Kubernetes
AppRole
AWS
Azure
```

---

## Secret Engines

Examples:

```text
KV
Database
PKI
Transit
AWS
Azure
SSH
```

---

# Prerequisites

```text
Linux Server
2 CPU
4 GB RAM
20 GB Disk
Port 8200 Open
```

---

# Step 1: Download Vault

```bash
wget https://releases.hashicorp.com/vault/1.19.0/vault_1.19.0_linux_amd64.zip
```

Extract:

```bash
unzip vault_1.19.0_linux_amd64.zip
```

Move Binary:

```bash
mv vault /usr/local/bin/
```

Verify:

```bash
vault version
```

---

# Step 2: Create Vault User

```bash
useradd vault
```

Create Directories:

```bash
mkdir -p /opt/vault/data
```

```bash
mkdir -p /etc/vault
```

---

# Step 3: Create Vault Configuration

```bash
vi /etc/vault/vault.hcl
```

Example:

```hcl
storage "raft" {
  path = "/opt/vault/data"
}

listener "tcp" {
  address     = "0.0.0.0:8200"
  tls_disable = 1
}

ui = true

api_addr = "http://0.0.0.0:8200"
```

---

# Step 4: Create Vault Service

```bash
vi /etc/systemd/system/vault.service
```

```ini
[Unit]
Description=HashiCorp Vault

[Service]
User=vault
ExecStart=/usr/local/bin/vault server \
-config=/etc/vault/vault.hcl

[Install]
WantedBy=multi-user.target
```

---

# Start Service

```bash
systemctl daemon-reload
```

```bash
systemctl enable vault
```

```bash
systemctl start vault
```

Verify:

```bash
systemctl status vault
```

---

# Step 5: Set Environment Variable

```bash
export VAULT_ADDR=http://127.0.0.1:8200
```

Verify:

```bash
vault status
```

---

# Step 6: Initialize Vault

```bash
vault operator init
```

Output:

```text
Unseal Key 1
Unseal Key 2
Unseal Key 3
Root Token
```

Save securely.

---

# Step 7: Unseal Vault

```bash
vault operator unseal
```

Enter Key 1

```bash
vault operator unseal
```

Enter Key 2

```bash
vault operator unseal
```

Enter Key 3

Check Status:

```bash
vault status
```

Expected:

```text
Sealed: false
```

---

# Step 8: Login

```bash
vault login
```

Paste Root Token.

Check Token:

```bash
vault token lookup
```

---

# Vault Administration Commands

---

# A. Authentication Commands

## Enable AppRole

```bash
vault auth enable approle
```

## Enable Kubernetes Auth

```bash
vault auth enable kubernetes
```

## Enable GitHub Auth

```bash
vault auth enable github
```

List Auth Methods:

```bash
vault auth list
```

---

# B. Backup Commands

## Snapshot Backup

```bash
vault operator raft snapshot save backup.snap
```

Restore:

```bash
vault operator raft snapshot restore backup.snap
```

---

# C. Configuration Commands

Show Configuration:

```bash
vault status
```

Health Check:

```bash
vault status -format=json
```

---

# D. Database Secrets

Enable:

```bash
vault secrets enable database
```

Configure:

```bash
vault write database/config/mysql
```

---

# E. Enable KV Secrets Engine

```bash
vault secrets enable -path=kv kv-v2
```

List Engines:

```bash
vault secrets list
```

---

# F. Fetch Secrets

Read Secret:

```bash
vault kv get kv/app
```

JSON Output:

```bash
vault kv get -format=json kv/app
```

---

# G. Generate Tokens

```bash
vault token create
```

Create Periodic Token:

```bash
vault token create -period=24h
```

---

# H. Health Check

```bash
vault status
```

API:

```bash
curl http://localhost:8200/v1/sys/health
```

---

# I. Identity Management

Create Entity:

```bash
vault write identity/entity \
name=developer
```

List Entities:

```bash
vault list identity/entity/id
```

---

# J. JSON Output

```bash
vault status -format=json
```

---

# K. KV Secrets

Store Secret:

```bash
vault kv put kv/app \
username=admin \
password=secret123
```

Read Secret:

```bash
vault kv get kv/app
```

Delete:

```bash
vault kv delete kv/app
```

---

# L. List Secrets

```bash
vault kv list kv/
```

---

# M. Metadata Commands

```bash
vault kv metadata get kv/app
```

---

# N. Namespaces (Enterprise)

Create Namespace:

```bash
vault namespace create prod
```

List:

```bash
vault namespace list
```

---

# O. Operators Commands

Status:

```bash
vault operator members
```

Step Down Leader:

```bash
vault operator step-down
```

---

# P. Policies

Create Policy:

```bash
vi dev-policy.hcl
```

```hcl
path "kv/*" {
  capabilities = ["read","list"]
}
```

Apply:

```bash
vault po*icy write dev dev-policy.hcl
```

*ist:

```bash
vault policy list
``*

Read:

```bash
vault policy read*dev
```

---

# Q. Query Secrets

*``bash
vault kv get kv/app
```

--*

# R. Raft Commands

Peers:

```b*sh
vault operator raft list-peers
*``

Join Cluster:

```bash
vault o*erator raft join http://10.0.0.10:*200
```

---

# S. Secret Engines
*List Engines:

```bash
vault secre*s list
```

Disable Engine:

```ba*h
vault secrets disable kv
```

--*

# T. Tokens

Create:

```bash
va*lt token create
```

Lookup:

```b*sh
vault token lookup
```

Revoke:*
```bash
vault token revoke TOKEN_*D
```

---

# U. Unseal Commands

*heck:

```bash
vault status
```

U*seal:

```bash
vault operator unse*l
```

Seal:

```bash
vault operat*r seal
```

---

# V. Version Info*mation

```bash
vault version
```
*---

# W. Write Secrets

```bash
v*ult kv put kv/app \
url=mysql.prod*local
```

---

# X. Export Secret*
```bash
vault kv get \
-format=js*n kv/app
```

---

# Y. YAML Integ*ation

## Kubernetes Secret Inject*on

```yaml
annotations:
  vault.h*shicorp.com/agent-inject: "true"
`*`

---

# Z. Advanced Administrati*n

Rotate Root Token:

```bash
vau*t token create
```

Rotate Encrypt*on Key:

```bash
vault operator ro*ate
```

Rotate Recovery Keys:

``*bash
vault operator rekey
*``

---

# Policy*Workflow

```text*Create Policy
      |
      ▼
Atta*h*Policy
      |
      ▼
Create Toke*
      |
      ▼
Access Secret
``*

---

# AppRole Workflow

Enable*

*``*ash
vault auth enable approle
```
*Create Role:

```*ash*vault write auth/approle/role/app-*ole \
policies=dev
```

*et Role ID:

```bash*vault read auth/approle/*ole/app-role/role-id
*``

Get Secret ID:

```bash*vault*write -f \
auth/*pprole/role/app-role/secret-id
```*
Login*

```bash*vault write auth/approle/login \
r*le_id=ROLE_ID \
secret_id=SECRET_I*
```

---

# Kubernetes Integratio*

Enable:

```bash
*ault auth enable kubernetes
```

C*nfigure:

```bash
vault write auth*kubernetes/config
```

*reate Role:

```bash*vault write auth/kubernetes/role/a*p
```

---

#*Kubernetes Secret Injection Flow

*``text
Application Pod
       |
  *    ▼
Service Account
       |
   *   ▼
Vault Auth
       |
       ▼
*olicy Check
       |
       ▼
KV S*cret Engine
       |
       ▼
Secr*t Mounted
```

---

#*CI/CD Workflow

```text*Developer
    *|
     ▼
GitHub*
     |
     ▼

Jenkins*Pipeline

     |
     ▼

Vault Aut*entication

     |
     ▼

Read Se*rets

     |
     ▼

Docker Build
*     |
     ▼

Kubernetes Deploy
`*`

---

# Monitoring Commands

Met*ics:

```bash*vault read sys/metrics
``*

*udit*Devices:

```bash
vault audit list*```

Enable Audit:

```bash
vault *udit enable file \
file_path=/*ar/log/vault_audit.log
```

*--

# Troubleshooting Commands

Ch*ck Status:

*``bash
vault status
```

*heck Logs:

```bash*journalctl -u vault -f
```

Port C*eck:

```bash
*s -tulnp | grep 820*
```

Health:

```bash*curl http*//localhost:8200/v1/sys/health
```*
Leader:

```bash
vault operator r*ft list-peers
``*

---

# Top Daily Vault Commands
*```bash
vault status
vault login
v*ult token lookup
vault token creat*
vault token revoke
vault policy*list
vault policy read
vault secre*s list
vault kv*put
vault kv get
vault kv list
vau*t operator init
vault operator*unseal
vault operator seal
vault o*erator raft list-peers
vault opera*or raft snapshot save
vault auth l*st
vault auth enable*approle
vault auth*enable kubernetes
vault audit list*vault version
```

*--

# Production Architecture

```*ext
Applications
      *|
       ▼
Load*Balancer
       |
       ▼*
+-------------+
| Vault*Node1 |
+-------------+

+--------*----+
| Vault Node2 |
+-----------*-+

+-------------+
| Vault Node3 *
+-------------+

       |
       *
 Raft Storage
```

*--

# End-to-End Vault Flow

```te*t*Application
     |
     ▼
Authenti*ate
     |
     ▼
Vault Token
    *|
     ▼
Policy Validation
     |
*    ▼
Secrets Engine
     |
     ▼*Secret Returned
     |
     ▼
Appl*cation Uses Secret
```

---

# Sum*ary

This guide covers:

- Vault I*stallation
- Initialization & Unse*l
- Authentication Methods
- KV Se*rets Engine
- Policies
- Tokens
- *ppRole Authentication
- Kubernetes*Authentication
- Audit Logging
- B*ckup & Restore
- Raft Clustering
-*Secret Injection
- DevOps CI/CD In*egration
- Production Administrati*n
- Troubleshooting

⭐ Keep this R*ADME as a complete HashiCorp Vault administration and secrets management reference for DevOps, SRE, Platform Engineering, Cloud Security, and Production Support.

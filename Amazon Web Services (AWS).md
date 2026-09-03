# Amazon Web Services (AWS)
# A to Z Guide: Architecture, Configuration, Workflow & Administration

A complete AWS Administrator, Cloud Engineer, DevOps Engineer, SRE and Solutions Architect guide covering:

- IAM
- EC2
- Auto Scaling
- S3
- EBS
- RDS
- VPC
- CloudWatch
- Route 53
- ECR
- EKS
- Security
- Monitoring
- Backup
- High Availability
- DevOps Integration
- Disaster Recovery
- Production Administration

---

# AWS Global Infrastructure

AWS Infrastructure consists of:

```text
Region

  |
  ├── Availability Zone A
  |
  ├── Availability Zone B
  |
  └── Availability Zone C
```

Example:

```text
ap-south-1 (Mumbai)

us-east-1 (N. Virginia)

eu-west-1 (Ireland)
```

---

# Enterprise AWS Architecture

```text
Users

   |
   ▼

Route53

   |
   ▼

Application Load Balancer

   |
   ▼

Auto Scaling Group

   |
   ▼

EC2 Instances

   |
   ▼

RDS Database

   |
   ▼

S3 Storage

   |
   ▼

CloudWatch Monitoring
```

---

# 1. IAM (Identity and Access Management)

## What is IAM?

IAM controls:

```text
Authentication

Authorization

Users

Roles

Policies
```

---

## IAM Architecture

```text
User

  |
  ▼

IAM Policy

  |
  ▼

AWS Resource
```

---

## Components

### Users

```text
Developers

Admins

Auditors
```

### Groups

```text
DevOps Team

Cloud Team

Security Team
```

### Roles

```text
EC2 Role

Lambda Role

EKS Role
```

### Policies

```json
{
  "Effect": "Allow",
  "Action": "*",
  "Resource": "*"
}
```

---

## IAM Best Practices

```text
Enable MFA

Use Roles

Avoid Root Usage

Apply Least Privilege
```

---

## IAM Commands

```bash
aws iam list-users

aws iam list-roles

aws iam create-user

aws iam create-role
```

---

# 2. EC2 (Elastic Compute Cloud)

## What is EC2?

EC2 provides virtual servers.

Use Cases:

```text
Web Servers

Database Servers

Jenkins Servers

Application Servers
```

---

## EC2 Architecture

```text
VPC

 |
 ▼

Subnet

 |
 ▼

EC2 Instance

 |
 ▼

EBS Volume
```

---

## Instance Types

### General Purpose

```text
t3.micro

t3.small
```

---

### Compute Optimized

```text
c5.large
```

---

### Memory Optimized

```text
r5.large
```

---

## EC2 Lifecycle

```text
Launch

Running

Stopping

Stopped

Terminated
```

---

## EC2 Commands

```bash
aws ec2 describe-instances

aws ec2 start-instances

aws ec2 stop-instances

aws ec2 terminate-instances
```

---

# 3. Auto Scaling

## What is Auto Scaling?

Automatically adds or removes EC2 instances.

---

## Architecture

```text
Load Balancer

      |
      ▼

Auto Scaling Group

      |
      ├── EC2-1
      ├── EC2-2
      └── EC2-3
```

---

## Scaling Policies

### Scale Out

```text
CPU > 70%
```

Add Server.

---

### Scale In

```text
CPU < 30%
```

Remove Server.

---

## Benefits

```text
Cost Optimization

High Availability

Automatic Recovery
```

---

# 4. Amazon S3

## What is Amazon S3?

Object Storage Service.

Stores:

```text
Images

Videos

Backups

Logs

Documents
```

---

## Architecture

```text
Bucket

 |
 ├── Images

 |
 ├── Videos

 |
 └── Logs
```

---

## Storage Classes

### Standard

```text
Frequently Accessed
```

---

### Standard IA

```text
Infrequently Accessed
```

---

### Glacier

```text
Long-Term Archive
```

---

## Commands

```bash
aws s3 ls

aws s3 cp

aws s3 sync

aws s3 rm
```

---

# 5. EBS (Elastic Block Store)

## What is EBS?

Persistent block storage attached to EC2.

---

## Architecture

```text
EC2

 |
 ▼

EBS Volume
```

---

## Volume Types

### General Purpose SSD

```text
gp3
```

---

### Provisioned IOPS

```text
io2
```

---

### Throughput Optimized HDD

```text
st1
```

---

## Commands

```bash
aws ec2 describe-volumes

aws ec2 create-volume
```

---

# 6. RDS (Relational Database Service)

## What is RDS?

Managed database service.

Supports:

```text
MySQL

PostgreSQL

MariaDB

Oracle

SQL Server
```

---

## Architecture

```text
Application

     |
     ▼

RDS

     |
     ▼

Storage
```

---

## Multi-AZ Setup

```text
Primary DB

      |
      ▼

Standby DB
```

Automatic Failover.

---

## Benefits

```text
Automated Backup

Patch Management

High Availability
```

---

## Commands

```bash
aws rds describe-db-instances

aws rds create-db-instance
```

---

# 7. VPC (Virtual Private Cloud)

## What is VPC?

Private network within AWS.

---

## Architecture

```text
VPC
 |
 ├── Public Subnet
 |
 └── Private Subnet
```

---

## Components

### Internet Gateway

```text
Internet Access
```

---

### Route Table

```text
Traffic Routing
```

---

### NAT Gateway

```text
Private Internet Access
```

---

### Security Groups

```text
Instance Firewall
```

---

### NACL

```text
Subnet Firewall
```

---

## Example

```text
10.0.0.0/16
```

---

# 8. Amazon CloudWatch

## What is CloudWatch?

Monitoring and observability service.

---

## Monitors

```text
CPU

Memory

Disk

Network

Application Metrics
```

---

## Architecture

```text
AWS Resources

      |
      ▼

CloudWatch

      |
      ▼

Alarms

      |
      ▼

SNS Notifications
```

---

## Common Metrics

```text
CPU Utilization

Disk Read

Disk Write

Network In

Network Out
```

---

## Commands

```bash
aws cloudwatch list-metrics

aws cloudwatch describe-alarms
```

---

# 9. Route 53

## What is Route 53?

AWS DNS Service.

---

## Architecture

```text
user

 |
 ▼

Route53

 |
 ▼

Load Balancer

 |
 ▼

Application
```

---

## Record Types

### A

```text
IPv4
```

### AAAA

```text
IPv6
```

### CNAME

```text
Alias
```

### MX

```text
Mail
```

### TXT

```text
Verification
```

---

## Features

```text
DNS Management

Health Checks

Traffic Routing

Failover Routing
```

---

# 10. ECR (Elastic Container Registry)

## What is ECR?

Private Docker Image Registry.

Stores:

```text
Docker Images

OCI Images
```

---

## Workflow

```text
Docker Build

     |
     ▼

ECR

     |
     ▼

EKS Pulls Image
```

---

## Commands

Login:

```bash
aws ecr get-login-password
```

Create Repository:

```bash
aws ecr create-repository \
--repository-name app
```

Push Image:

```bash
docker push
```

---

# 11. EKS (Elastic Kubernetes Service)

## What is EKS?

Managed Kubernetes Service.

---

## Architecture

```text
EKS Cluster

    |
    ├── Control Plane
    |
    └── Worker Nodes

           |
           ▼

          Pods
```

---

## Workflow

```text
Docker Build

      |
      ▼

ECR

      |
      ▼

EKS

      |
      ▼

Pods Running
```

---

## Commands

```bash
aws eks list-clusters

kubectl get nodes

kubectl get pods

kubectl get deployments
```

---

# AWS Networking Flow

```text
Internet

   |
   ▼

Route53

   |
   ▼

Application Load Balancer

   |
   ▼

Auto Scaling Group

   |
   ▼

EC2 Instances

   |
   ▼

RDS Database
```

---

# AWS Security Architecture

```text
IAM

   |
   ▼

Users

   |
   ▼

Roles

   |
   ▼

Policies

   |
   ▼

Resources
```

---

# AWS Monitoring Architecture

```text
EC2
RDS
EKS
S3

 |
 ▼

CloudWatch

 |
 ▼

Alarms

 |
 ▼

SNS
```

---

# AWS DevOps Workflow

```text
Developer

    |
    ▼

GitHub

    |
    ▼

Jenkins

    |
    ▼

Build Docker Image

    |
    ▼

ECR

    |
    ▼

EKS Deployment

    |
    ▼

CloudWatch Monitoring
```

---

# Disaster Recovery

## Backup Services

```text
S3 Versioning

RDS Snapshot

EBS Snapshot

AWS Backup
```

---

## HA Architecture

```text
Load Balancer

      |
      ▼

AZ-1 EC2

AZ-2 EC2

AZ-3 EC2

      |
      ▼

Multi-AZ RDS
```

---

# AWS Administrator Daily Commands

## Configure CLI

```bash
aws configure
```

---

## EC2

```bash
aws ec2 describe-instances
```

---

## S3

```bash
aws s3 ls
```

---

## IAM

```bash
aws iam list-users
```

---

## RDS

```bash
aws rds describe-db-instances
```

---

## EKS

```bash
kubectl get pods -A
```

---

## ECR

```bash
aws ecr describe-repositories
```

---

## CloudWatch

```bash
aws cloudwatch list-metrics
```

---

# Production Best Practices

## Security

```text
Enable MFA

Use IAM Roles

Rotate Keys

Encrypt Data
```

---

## Networking

```text
Private Subnets

NAT Gateway

Security Groups

NACLs
```

---

## Storage

```text
Enable S3 Versioning

Enable Lifecycle Policies

EBS Snapshots
```

---

## Monitoring

```text
CloudWatch

CloudTrail

AWS Config
```

---

## Kubernetes

```text
Use Managed Node Groups

Use ECR

Enable Auto Scaling

Enable Monitoring
```

---

# Complete Enterprise Architecture

```text
Users
   |
   ▼
Route53
   |
   ▼
Application Load Balancer
   |
   ▼
Auto Scaling Group
   |
   ▼
EC2 / EKS
   |
   ▼
ECR
   |
   ▼
RDS
   |
   ▼
S3
   |
   ▼
CloudWatch
   |
   ▼
SNS Alerts
```

---

# Summary

## Identity & Security

```text
IAM
```

## Compute

```text
EC2
Auto Scaling
```

## Storage

```text
S3
EBS
```

## Database

```text
RDS
```

## Networking

```text
VPC
Route53
```

## Monitoring

```text
CloudWatch
```

## Containers

```text
ECR
EKS
```

✅ AWS Architecture

✅ Configuration

✅ Administration

✅ Monitoring

✅ Security

✅ DevOps Integration

✅ Kubernetes

✅ High Availability

✅ Disaster Recovery

✅ Production Operations

⭐ This README serves as a complete AWS Cloud Administrator, DevOps Engineer, SRE, Cloud Architect, and Production Support reference guide.

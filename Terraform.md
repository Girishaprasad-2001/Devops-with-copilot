# Terraform A to Z Guide
## Architecture, Configuration, Workflow & Administration

A complete Terraform reference for:

- DevOps Engineers
- Cloud Engineers
- Platform Engineers
- SREs
- Infrastructure Administrators
- Cloud Architects

This guide covers:

- Terraform Fundamentals
- Installation
- Providers
- Resources
- Variables
- Outputs
- Modules
- Terraform State
- Remote Backend
- Workspaces
- Terraform Commands
- AWS Infrastructure Provisioning
- Kubernetes Provisioning
- CI/CD Integration
- Security Best Practices
- Troubleshooting

---

# What is Terraform?

Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp.

Terraform allows you to:

```text
Create Infrastructure
Modify Infrastructure
Destroy Infrastructure
Automate Cloud Resources
Manage Multi-Cloud Environments
```

Supported Platforms:

```text
AWS
Azure
GCP
VMware
Kubernetes
Oracle Cloud
OpenStack
Docker
GitHub
```

---

# Infrastructure as Code (IaC)

Traditional Approach:

```text
Login Cloud Console
Create VM
Configure Network
Create Database
```

Terraform Approach:

```text
Write Code
terraform apply

Infrastructure Created Automatically
```

---

# Terraform Architecture

```text
Terraform Code
      |
      ▼

Terraform CLI
      |
      ▼

Provider

      |
      ▼

Cloud API

      |
      ▼

Infrastructure
```

---

# Terraform Workflow

```text
Write Code

      |
      ▼

terraform init

      |
      ▼

terraform validate

      |
      ▼

terraform plan

      |
      ▼

terraform apply

      |
      ▼

Infrastructure Created
```

---

# Terraform Components

## Provider

Provider connects Terraform with external systems.

Example:

```hcl
provider "aws" {

  region = "ap-south-1"

}
```

---

## Resource

Represents infrastructure.

Example:

```hcl
resource "aws_instance" "web" {

}
```

---

## Variables

Used for reusable configuration.

Example:

```hcl
variable "instance_type" {

}
```

---

## Output

Displays created information.

Example:

```hcl
output "public_ip" {

}
```

---

## Module

Reusable Terraform package.

Example:

```hcl
module "network" {

}
```

---

# Terraform Installation

## Linux

Download Terraform:

```bash
wget https://releases.hashicorp.com/terraform/
```

Extract:

```bash
unzip terraform.zip
```

Move Binary:

```bash
sudo mv terraform /usr/local/bin/
```

Verify:

```bash
terraform version
```

---

# Terraform Directory Structure

```text
terraform-project/

├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
├── provider.tf
└── backend.tf
```

---

# First Terraform Configuration

## provider.tf

```hcl
provider "aws" {

  region = "ap-south-1"

}
```

---

# Create EC2 Instance

## main.tf

```hcl
resource "aws_instance" "webserver" {

  ami = "ami-xxxxxxxx"

  instance_type = "t2.micro"

}
```

---

# Initialize Project

```bash
terraform init
```

Output:

```text
Initializing provider plugins
```

---

# Validate Configuration

```bash
terraform validate
```

Output:

```text
Success
```

---

# Format Code

```bash
terraform fmt
```

---

# Plan Deployment

```bash
terraform plan
```

Purpose:

```text
Preview Changes
```

---

# Apply Configuration

```bash
terraform apply
```

Approve:

```text
yes
```

---

# Destroy Infrastructure

```bash
terraform destroy
```

---

# Variables

## variables.tf

```hcl
variable "instance_type" {

  default = "t2.micro"

}
```

---

## Use Variable

```hcl
instance_type = var.instance_type
```

---

# Variable Values

## terraform.tfvars

```hcl
instance_type = "t3.micro"
```

---

# Outputs

## outputs.tf

```hcl
output "instance_id" {

  value = aws_instance.webserver.id

}
```

---

# Terraform State

Terraform tracks resources using:

```text
terraform.tfstate
```

Purpose:

```text
Current Infrastructure State
```

---

# State Workflow

```text
Terraform

      |
      ▼

terraform.tfstate

      |
      ▼

Actual Cloud Resources
```

---

# State Commands

## Show State

```bash
terraform show
```

---

## List Resources

```bash
terraform state list
```

---

## Inspect Resource

```bash
terraform state show aws_instance.webserver
```

---

# Remote Backend

Store state remotely.

Benefits:

```text
Team Collaboration
State Locking
Security
```

---

# AWS S3 Backend

## backend.tf

```hcl
terraform {

  backend "s3" {

    bucket = "terraform-state-bucket"

    key = "prod/terraform.tfstate"

    region = "ap-south-1"

  }

}
```

---

# Terraform Workspaces

Create Environment Separation:

```text
dev
test
prod
```

---

## Create Workspace

```bash
terraform workspace new dev
```

---

## List Workspaces

```bash
terraform workspace list
```

---

## Select Workspace

```bash
terraform workspace select prod
```

---

# Modules

Reusable Infrastructure Components.

---

## Module Structure

```text
modules/

└── ec2

    ├── main.tf
    ├── variables.tf
    └── outputs.tf
```

---

# Call Module

```hcl
module "ec2" {

  source = "./modules/ec2"

}
```

---

# Data Sources

Retrieve Existing Infrastructure.

Example:

```hcl
data "aws_vpc" "default" {

  default = true

}
```

---

# Terraform Functions

## Uppercase

```hcl
upper("terraform")
```

Output:

```text
TERRAFORM
```

---

## Length

```hcl
length(var.list)
```

---

# Terraform Expressions

```hcl
count = 3
```

Creates:

```text
3 Resources
```

---

# Count Example

```hcl
resource "aws_instance" "web" {

  count = 3

}
```

---

# For Each Example

```hcl
resource "aws_s3_bucket" "bucket" {

  for_each = toset(["dev","test","prod"])

}
```

---

# AWS EC2 Example

```hcl
resource "aws_instance" "server" {

  ami = "ami-123456"

  instance_type = "t2.micro"

  tags = {

      Name = "terraform-server"

  }

}
```

---

# AWS Security Group

```hcl
resource "aws_security_group" "web" {

  name = "web-sg"

}
```

---

# AWS VPC

```hcl
resource "aws_vpc" "main" {

  cidr_block = "10.0.0.0/16"

}
```

---

# Kubernetes Provider

```hcl
provider "kubernetes" {

}
```

---

# Deploy Namespace

```hcl
resource "kubernetes_namespace" "dev" {

  metadata {

     name = "dev"

  }

}
```

---

# Docker Provider

```hcl
provider "docker" {}
```

---

# Docker Container

```hcl
resource "docker_container" "nginx" {

  image = "nginx"

  name  = "nginx"

}
```

---

# CI/CD Workflow

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

Terraform Init

   |
   ▼

Terraform Plan

   |
   ▼

Approval

   |
   ▼

Terraform Apply

   |
   ▼

Infrastructure Created
```

---

# GitHub Actions Workflow

```yaml
name: Terraform

on:
  push:

jobs:

  terraform:

    runs-on: ubuntu-latest

    steps:

    - uses: actions/checkout@v4

    - name: Terraform Init

      run: terraform init

    - name: Terraform Plan

      run: terraform plan

    - name: Terraform Apply

      run: terraform apply -auto-approve
```

---

# Security Best Practices

## Use Remote State

```text
S3 Backend
Azure Storage
GCS Backend
```

---

## Enable State Locking

```text
DynamoDB
```

---

## Never Store Secrets

Avoid:

```hcl
password = "admin123"
```

Use:

```text
HashiCorp Vault
AWS Secrets Manager
Azure Key Vault
```

---

# Common Terraform Commands

## Version

```bash
terraform version
```

---

## Initialize

```bash
terraform init
```

---

## Validate

```bash
terraform validate
```

---

## Format

```bash
terraform fmt
```

---

## Plan

```bash
terraform plan
```

---

## Apply

```bash
terraform apply
```

---

## Destroy

```bash
terraform destroy
```

---

## Show

```bash
terraform show
```

---

## Refresh

```bash
terraform refresh
```

---

## Output

```bash
terraform output
```

---

## Graph

```bash
terraform graph
```

---

# Troubleshooting Commands

## Validate Config

```bash
terraform validate
```

---

## View State

```bash
terraform show
```

---

## Debug Logs

```bash
export TF_LOG=DEBUG
```

```bash
terraform apply
```

---

## Provider Check

```bash
terraform providers
```

---

# Daily Terraform Commands

```bash
terraform init

terraform validate

terraform fmt

terraform plan

terraform apply

terraform destroy

terraform output

terraform show

terraform state list

terraform workspace list

terraform workspace select dev
```

---

# Enterprise Terraform Architecture

```text
Developer
      |
      ▼

GitHub

      |
      ▼

Jenkins/GitHub Actions

      |
      ▼

Terraform

      |
      ▼

Remote State (S3)

      |
      ▼

AWS/Azure/GCP

      |
      ▼

Infrastructure
```

---

# End-to-End Terraform Workflow

```text
Write Terraform Code

         |
         ▼

terraform init

         |
         ▼

terraform validate

         |
         ▼

terraform fmt

         |
         ▼

terraform plan

         |
         ▼

Review Changes

         |
         ▼

terraform apply

         |
         ▼

Infrastructure Created

         |
         ▼

terraform destroy
```

---

# Interview Questions

### What is Terraform?

```text
Infrastructure as Code Tool
```

### What is State File?

```text
terraform.tfstate
```

### What is Provider?

```text
Cloud/API Integration Plugin
```

### Difference Between Count and For_Each?

```text
Count Uses Numbers
For_each Uses Collections
```

### What is Terraform Backend?

```text
Remote State Storage
```

---

# Summary

This guide covers:

✅ Terraform Fundamentals

✅ Installation

✅ Providers

✅ Resources

✅ Variables

✅ Outputs

✅ State Management

✅ Remote Backend

✅ Modules

✅ Workspaces

✅ AWS Infrastructure

✅ Kubernetes Resources

✅ Docker Resources

✅ CI/CD Integration

✅ Security Best Practices

✅ Troubleshooting

✅ Production Administration

⭐ Keep this README as a complete Terraform reference for DevOps Engineers, Cloud Engineers, SREs, Platform Engineers, Solution Architects, and Infrastructure Administrators.

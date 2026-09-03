# Terraform Provider Configuration for Azure & AWS
## A to Z Setup Guide (README.md)

---

# Overview

This guide explains how to configure Terraform providers for:

- AWS
- Microsoft Azure
- Multi-Cloud Deployments
- Remote State Management
- Authentication Methods
- Security Best Practices
- CI/CD Integration
- Production Deployments

---

# What is a Terraform Provider?

A Provider is a plugin that enables Terraform to communicate with APIs of cloud platforms and services.

Architecture:

```text
Terraform Code
      |
      ▼
Terraform Provider
      |
      ▼
Cloud API
      |
      ▼
Infrastructure Resources
```

Examples:

```text
AWS Provider

AzureRM Provider

Kubernetes Provider

Docker Provider

GitHub Provider
```

---

# Terraform Provider Architecture

```text
Developer

     |
     ▼

Terraform Files

     |
     ▼

Providers

     |
     ├── AWS Provider
     |
     └── Azure Provider

     |
     ▼

Cloud APIs

     |
     ▼

AWS Resources

Azure Resources
```

---

# Prerequisites

Install Terraform:

```bash
terraform version
```

Install AWS CLI:

```bash
aws --version
```

Install Azure CLI:

```bash
az version
```

Verify:

```bash
terraform version

aws sts get-caller-identity

az account show
```

---

# Project Structure

```text
terraform-multicloud/

├── provider.tf
├── variables.tf
├── terraform.tfvars
├── main.tf
├── outputs.tf
├── backend.tf
└── versions.tf
```

---

# AWS Provider Configuration

## Step 1: Create IAM User

Navigate:

```text
AWS Console

   |
   ▼

IAM

   |
   ▼

Users

   |
   ▼

Create User
```

Assign:

```text
Programmatic Access
```

Permissions:

```text
AdministratorAccess
```

For production:

```text
Least Privilege Policy
```

---

# Step 2: Configure AWS CLI

Configure:

```bash
aws configure
```

Enter:

```text
AWS Access Key ID

AWS Secret Access Key

Region

Output Format
```

Example:

```text
AWS Access Key ID     : AKIAXXXXXXXX

AWS Secret Access Key : xxxxxxxxxxxx

Region                : ap-south-1

Output Format         : json
```

Verify:

```bash
aws sts get-caller-identity
```

---

# Step 3: Configure AWS Provider

## versions.tf

```hcl
terraform {

  required_version = ">= 1.5"

  required_providers {

    aws = {

      source  = "hashicorp/aws"

      version = "~> 5.0"

    }

  }

}
```

---

## provider.tf

```hcl
provider "aws" {

  region = "ap-south-1"

}
```

---

# AWS Provider Using Access Keys

```hcl
provider "aws" {

  region     = "ap-south-1"

  access_key = "ACCESS_KEY"

  secret_key = "SECRET_KEY"

}
```

⚠️ Not Recommended.

---

# AWS Provider Using Environment Variables

Linux:

```bash
export AWS_ACCESS_KEY_ID=AKIAxxxx

export AWS_SECRET_ACCESS_KEY=xxxxxxxx

export AWS_DEFAULT_REGION=ap-south-1
```

Terraform:

```hcl
provider "aws" {}
```

---

# AWS EC2 Example

## main.tf

```hcl
resource "aws_instance" "web" {

  ami           = "ami-0f58b397bc5c1f2e8"

  instance_type = "t2.micro"

  tags = {

    Name = "terraform-aws-server"

  }

}
```

---

# Azure Provider Configuration

---

# Step 1: Login to Azure

```bash
az login
```

Verify:

```bash
az account show
```

---

# Step 2: Get Subscription ID

```bash
az account show
```

Output:

```json
{
  "id": "subscription-id"
}
```

Save:

```text
Subscription ID

Tenant ID
```

---

# Step 3: Create Service Principal

```bash
az ad sp create-for-rbac \
--name terraform-sp
```

Output:

```json
{
  "appId": "client-id",

  "password": "client-secret",

  "tenant": "tenant-id"
}
```

---

# Step 4: Configure Azure Provider

## versions.tf

```hcl
terraform {

  required_providers {

    azurerm = {

      source  = "hashicorp/azurerm"

      version = "~>4.0"

    }

  }

}
```

---

## provider.tf

```hcl
provider "azurerm" {

  features {}

  subscription_id = "subscription-id"

  tenant_id       = "tenant-id"

  client_id       = "client-id"

  client_secret   = "client-secret"

}
```

---

# Azure Provider Using Environment Variables

```bash
export ARM_SUBSCRIPTION_ID="subscription-id"

export ARM_CLIENT_ID="client-id"

export ARM_CLIENT_SECRET="client-secret"

export ARM_TENANT_ID="tenant-id"
```

Provider:

```hcl
provider "azurerm" {

  features {}

}
```

---

# Azure Resource Group Example

```hcl
resource "azurerm_resource_group" "rg" {

  name     = "terraform-rg"

  location = "Central India"

}
```

---

# Multi-Cloud Configuration

## versions.tf

```hcl
terraform {

  required_version = ">=1.5"

  required_providers {

    aws = {

      source  = "hashicorp/aws"

      version = "~>5.0"

    }

    azurerm = {

      source  = "hashicorp/azurerm"

      version = "~>4.0"

    }

  }

}
```

---

# Multi-Cloud Providers

## provider.tf

```hcl
provider "aws" {

  region = "ap-south-1"

}

provider "azurerm" {

  features {}

}
```

---

# Multi-Cloud Example

```hcl
resource "aws_instance" "aws_server" {

  ami           = "ami-0f58b397bc5c1f2e8"

  instance_type = "t2.micro"

}
```

---

```hcl
resource "azurerm_resource_group" "rg" {

  name     = "terraform-rg"

  location = "Central India"

}
```

Apply:

```bash
terraform apply
```

Result:

```text
AWS Resource Created

Azure Resource Created
```

---

# Variables

## variables.tf

```hcl
variable "aws_region" {

  default = "ap-south-1"

}

variable "resource_group_name" {

  default = "terraform-rg"

}
```

---

# terraform.tfvars

```hcl
aws_region = "ap-south-1"

resource_group_name = "terraform-rg"
```

---

# Outputs

## outputs.tf

```hcl
output "resource_group_name" {

  value = azurerm_resource_group.rg.name

}
```

---

# Initialize Providers

```bash
terraform init
```

Downloads:

```text
AWS Provider

AzureRM Provider
```

---

# Validate Configuration

```bash
terraform validate
```

---

# Format Code

```bash
terraform fmt
```

---

# Preview Changes

```bash
terraform plan
```

---

# Deploy Resources

```bash
terraform apply
```

---

# Destroy Resources

```bash
terraform destroy
```

---

# Provider Management Commands

## Show Installed Providers

```bash
terraform providers
```

---

## Upgrade Providers

```bash
terraform init -upgrade
```

---

## Lock Provider Versions

```bash
terraform providers lock
```

---

# AWS Remote Backend

## backend.tf

```hcl
terraform {

  backend "s3" {

    bucket = "terraform-state-prod"

    key = "prod/terraform.tfstate"

    region = "ap-south-1"

    encrypt = true

  }

}
```

---

# Azure Remote Backend

```hcl
terraform {

  backend "azurerm" {

    resource_group_name  = "tf-rg"

    storage_account_name = "tfstorageacct"

    container_name       = "tfstate"

    key                  = "prod.tfstate"

  }

}
```

---

# State Management Workflow

```text
Terraform Code

      |
      ▼

Terraform Apply

      |
      ▼

Terraform State

      |
      ▼

AWS S3

or

Azure Storage
```

---

# Authentication Best Practices

## AWS

Recommended:

```text
IAM Roles

Instance Profiles

OIDC

AWS SSO
```

Avoid:

```text
Static Keys
```

---

## Azure

Recommended:

```text
Managed Identity

Service Principal

Workload Identity
```

Avoid:

```text
Hardcoded Secrets
```

---

# Azure DevOps Integration

Example:

```yaml
- task: TerraformTaskV4@4

  inputs:

    provider: azurerm

    command: apply
```

---

# GitHub Actions Integration

```yaml
- name: Terraform Init

  run: terraform init

- name: Terraform Plan

  run: terraform plan

- name: Terraform Apply

  run: terraform apply -auto-approve
```

---

# Enterprise Multi-Cloud Workflow

```text
Developer

      |
      ▼

GitHub / Azure Repos

      |
      ▼

Azure DevOps

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

AWS Resources

Azure Resources
```

---

# Common Terraform Commands

```bash
terraform init

terraform validate

terraform fmt

terraform plan

terraform apply

terraform destroy

terraform output

terraform show

terraform providers

terraform workspace list

terraform workspace select dev
```

---

# Troubleshooting

## Check Provider Installation

```bash
terraform providers
```

---

## Check Authentication

AWS:

```bash
aws sts get-caller-identity
```

Azure:

```bash
az account show
```

---

## Debug Logs

```bash
export TF_LOG=DEBUG
```

Linux:

```bash
terraform apply
```

---

# Security Best Practices

✅ Use Remote State

✅ Enable State Encryption

✅ Enable State Locking

✅ Use IAM Roles

✅ Use Azure Managed Identity

✅ Store Secrets in Key Vault

✅ Store Secrets in AWS Secrets Manager

✅ Never Commit Secrets to Git

✅ Use Terraform Workspaces

✅ Use Provider Version Pinning

---

# Interview Questions

### What is a Terraform Provider?

```text
A plugin that enables Terraform
to interact with cloud APIs.
```

### AWS Provider Name?

```text
hashicorp/aws
```

### Azure Provider Name?

```text
hashicorp/azurerm
```

### Command to Download Providers?

```bash
terraform init
```

### Best Production Authentication?

```text
AWS IAM Roles

Azure Managed Identities
```

---

# Summary

✅ AWS Provider Configuration

✅ Azure Provider Configuration

✅ Multi-Cloud Deployment

✅ Authentication Methods

✅ Environment Variables

✅ EC2 Example

✅ Azure Resource Group Example

✅ State Management

✅ Remote Backends

✅ CI/CD Integration

✅ Azure DevOps Integration

✅ GitHub Actions

✅ Security Best Practices

✅ Enterprise Multi-Cloud Architecture

⭐ Keep this README as a complete Terraform Provider Configuration reference for AWS, Azure, DevOps, Cloud Engineering, SRE, and Platform Engineering teams.

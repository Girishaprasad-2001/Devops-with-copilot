# Azure DevOps & Service Connections
# A to Z Guide: Architecture, Configuration, Workflow & Administration

A complete Azure DevOps Administrator and DevOps Engineer Guide covering:

- Azure DevOps Overview
- Azure Repos
- Azure Pipelines
- Azure Boards
- Azure Artifacts
- Azure Test Plans
- Service Connections
- CI/CD Pipelines
- Security & Permissions
- Environments
- Deployment Strategies
- Azure Integration
- AWS Integration
- Kubernetes Integration
- Terraform Integration
- GitHub Integration
- Monitoring
- Troubleshooting
- Best Practices

---

# What is Azure DevOps?

Azure DevOps is Microsoft's DevOps Platform providing:

```text
Source Code Management

CI/CD Pipelines

Agile Planning

Package Management

Test Management

Release Management
```

---

# Azure DevOps Services

```text
Azure Boards

Azure Repos

Azure Pipelines

Azure Test Plans

Azure Artifacts
```

---

# Azure DevOps Architecture

```text
Developer

    |
    ▼

Azure Repos

    |
    ▼

Azure Pipelines

    |
    ▼

Build

    |
    ▼

Artifact

    |
    ▼

Deployment

    |
    ▼

Azure / AWS / Kubernetes
```

---

# Azure DevOps Workflow

```text
Developer

     |
     ▼

Git Commit

     |
     ▼

Azure Repos

     |
     ▼

Pipeline Trigger

     |
     ▼

Build

     |
     ▼

Testing

     |
     ▼

Artifact Creation

     |
     ▼

Deployment

     |
     ▼

Production
```

---

# Azure Boards

## What is Azure Boards?

Work Tracking System.

Used For:

```text
Epics

Features

User Stories

Tasks

Bugs

Sprint Planning
```

---

# Boards Hierarchy

```text
Epic

 |
 ▼

Feature

 |
 ▼

User Story

 |
 ▼

Task
```

---

# Azure Repos

## What is Azure Repos?

Git-based Source Code Repository.

Supports:

```text
Git

Branch Policies

Pull Requests

Code Reviews
```

---

# Repository Workflow

```text
Developer

      |
      ▼

Feature Branch

      |
      ▼

Pull Request

      |
      ▼

Code Review

      |
      ▼

Merge

      |
      ▼

Main Branch
```

---

# Azure Pipelines

## What is Azure Pipelines?

CI/CD Service.

Performs:

```text
Build

Unit Testing

Static Analysis

Container Build

Deployment
```

---

# Pipeline Architecture

```text
Source Code

      |
      ▼

Build Agent

      |
      ▼

Build Stage

      |
      ▼

Test Stage

      |
      ▼

Deploy Stage
```

---

# Azure Artifacts

## What is Azure Artifacts?

Package Repository.

Stores:

```text
NuGet

Maven

NPM

Python Packages

Universal Packages
```

---

# Azure Test Plans

## What is Test Plans?

Test Management Tool.

Provides:

```text
Manual Testing

Test Cases

Regression Testing

Exploratory Testing
```

---

# What is a Service Connection?

A Service Connection allows Azure DevOps Pipelines to securely connect to external systems.

Examples:

```text
Azure Subscription

Kubernetes Cluster

Docker Registry

GitHub

AWS

Terraform Cloud

ServiceNow
```

---

# Service Connection Architecture

```text
Azure Pipeline

       |
       ▼

Service Connection

       |
       ▼

Target Platform

       |
       ▼

Deployment
```

---

# Why Service Connections?

Without Service Connection:

```text
Manual Authentication
```

With Service Connection:

```text
Automated Authentication

Secure Deployments
```

---

# Service Connection Types

## Azure Resource Manager

```text
Azure Subscription Access
```

---

## Kubernetes

```text
AKS Access

Kubernetes Access
```

---

## Docker Registry

```text
Docker Hub

Azure Container Registry

JFrog
```

---

## GitHub

```text
Source Code Access
```

---

## AWS

```text
AWS Resource Access
```

---

## Generic Service

```text
Custom API Access
```

---

# Azure Resource Manager Connection

## Purpose

Connect Azure DevOps to Azure Subscription.

Used For:

```text
VM Deployment

AKS Deployment

Terraform

App Service Deployment
```

---

# Azure Connection Workflow

```text
Azure Pipeline

      |
      ▼

Azure Service Principal

      |
      ▼

Azure Subscription

      |
      ▼

Deployment
```

---

# Create Azure Service Connection

Navigate:

```text
Project Settings

      |
      ▼

Service Connections

      |
      ▼

New Service Connection

      |
      ▼

Azure Resource Manager
```

---

# Authentication Methods

## Automatic

Recommended.

```text
Azure DevOps Creates
Service Principal
```

---

## Manual

```text
Client ID

Client Secret

Tenant ID

Subscription ID
```

---

# Kubernetes Service Connection

## Purpose

Deploy Applications into Kubernetes.

Supported:

```text
AKS

EKS

OpenShift

On-prem Kubernetes
```

---

# Workflow

```text
Azure Pipelines

       |
       ▼

Kubernetes Service Connection

       |
       ▼

kubectl Deployment

       |
       ▼

Cluster
```

---

# Docker Registry Service Connection

## Used For

```text
ECR

ACR

Docker Hub

JFrog Artifactory
```

---

# Workflow

```text
Pipeline

   |
   ▼

Docker Login

   |
   ▼

Push Image

   |
   ▼

Registry
```

---

# GitHub Service Connection

## Purpose

Connect:

```text
GitHub Repository

GitHub Enterprise
```

---

# Workflow

```text
Azure Pipeline

     |
     ▼

GitHub Repository

     |
     ▼

Build Trigger
```

---

# AWS Service Connection

## Purpose

Deploy Resources To AWS.

Examples:

```text
EC2

EKS

S3

Lambda
```

---

# AWS Authentication

```text
Access Key

Secret Key
```

---

# Terraform Service Connection

## Purpose

Authenticate Terraform.

Workflow:

```text
Terraform

    |
    ▼

Azure Service Connection

    |
    ▼

Azure Infrastructure
```

---

# Pipeline Agents

## Microsoft Hosted Agent

Prebuilt Agent.

Examples:

```text
ubuntu-latest

windows-latest

macOS-latest
```

---

## Self Hosted Agent

Installed On:

```text
Linux

Windows

VMs

Kubernetes
```

---

# Self Hosted Agent Setup

Install Agent:

```bash
mkdir agent

cd agent
```

Download:

```bash
wget agent-package
```

Configure:

```bash
./config.sh
```

Start:

```bash
./run.sh
```

---

# Azure DevOps Pipeline YAML

## Build Example

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:

- task: Maven@4

  inputs:
    goals: clean package
```

---

# Multi-Stage Pipeline

```yaml
stages:

- stage: Build

- stage: Test

- stage: Security

- stage: Deploy
```

---

# Azure App Service Deployment

```yaml
- task: AzureWebApp@1

  inputs:

    azureSubscription: 'Prod-Connection'

    appName: 'myapp'

    package: '*.zip'
```

---

# AKS Deployment Pipeline

```yaml
- task: KubernetesManifest@1

  inputs:

    action: deploy

    kubernetesServiceConnection: aks-prod

    manifests: deployment.yaml
```

---

# Docker Build Pipeline

```yaml
- task: Docker@2

  inputs:

    command: buildAndPush

    repository: myapp
```

---

# Environment Management

## Purpose

Deployment Control.

Examples:

```text
DEV

TEST

UAT

PROD
```

---

# Deployment Workflow

```text
Build

   |
   ▼

Dev

   |
   ▼

QA

   |
   ▼

UAT

   |
   ▼

Production
```

---

# Approvals and Gates

Used For:

```text
Production Approval

CAB Approval

Security Approval
```

---

# Security Architecture

```text
Azure AD

    |
    ▼

Azure DevOps

    |
    ▼

RBAC

    |
    ▼

Projects
```

---

# Azure DevOps Roles

```text
Project Administrator

Project Contributor

Project Reader

Build Administrator

Release Administrator
```

---

# Secrets Management

Options:

```text
Azure Key Vault

Variable Groups

Pipeline Secrets
```

---

# Azure Key Vault Integration

```yaml
- task: AzureKeyVault@2

  inputs:

    azureSubscription: prod

    KeyVaultName: prod-kv
```

---

# CI/CD Enterprise Workflow

```text
Developer

      |
      ▼

Azure Repos

      |
      ▼

Pull Request

      |
      ▼

Build

      |
      ▼

SonarQube

      |
      ▼

Docker Build

      |
      ▼

Container Registry

      |
      ▼

AKS Deployment

      |
      ▼

Production
```

---

# Monitoring Pipeline Execution

Navigate:

```text
Pipelines

      |
      ▼

Runs

      |
      ▼

Logs
```

---

# Troubleshooting

## Agent Issues

Check:

```bash
./svc.sh status
```

---

## Verify Pipeline

```text
Pipeline Logs
```

---

## Service 

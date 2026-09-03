# Trivy Image Scanning
# A to Z Guide: Architecture, Configuration, Workflow & Administration

A complete Trivy Security Scanning reference covering:

- Trivy Fundamentals
- Container Image Scanning
- File System Scanning
- Kubernetes Scanning
- Infrastructure as Code (IaC) Scanning
- Secret Scanning
- SBOM Generation
- CI/CD Integration
- Docker Integration
- GitHub Actions
- Jenkins Integration
- Kubernetes Security
- Vulnerability Management
- Compliance
- Reporting
- Troubleshooting

---

# What is Trivy?

Trivy is an Open Source Security Scanner developed by Aqua Security.

Trivy scans:

```text
Docker Images
Container Images
Kubernetes Clusters
Terraform Code
Kubernetes YAML Files
Git Repositories
Secrets
Operating Systems
Programming Dependencies
```

---

# Why Trivy?

Benefits:

```text
Fast Scanning

Easy Installation

Container Security

Kubernetes Security

IaC Security

SBOM Generation

Open Source
```

---

# Trivy Architecture

```text
Application Source
        |
        ▼

Docker Image

        |
        ▼

Trivy Scanner

        |
        ▼

Vulnerability Database

        |
        ▼

Security Report

        |
        ▼

DevOps Team
```

---

# Security Workflow

```text
Developer

      |
      ▼

Code Commit

      |
      ▼

Build Image

      |
      ▼

Trivy Scan

      |
      ▼

Critical Check

      |
      ▼

Deploy

      |
      ▼

Production
```

---

# Vulnerability Scan Flow

```text
Container Image

       |
       ▼

Operating System Packages

       |
       ▼

Libraries

       |
       ▼

Known CVEs

       |
       ▼

Scan Results
```

---

# Trivy Installation

## Linux

Install:

```bash
curl -sfL \
https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh \
| sudo sh -s -- -b /usr/local/bin
```

Verify:

```bash
trivy --version
```

---

# Docker Installation

```bash
docker pull aquasec/trivy
```

Run:

```bash
docker run aquasec/trivy image nginx
```

---

# Verify Installation

```bash
trivy --version
```

Example:

```text
Version: 0.xx.x
```

---

# Trivy Scan Types

```text
Image Scan

Filesystem Scan

Repository Scan

Kubernetes Scan

Secret Scan

SBOM Scan

Configuration Scan
```

---

# Docker Image Scanning

## Scan Nginx Image

```bash
trivy image nginx:latest
```

---

## Scan Local Image

```bash
trivy image myapp:v1
```

---

# Example Output

```text
CRITICAL: 3

HIGH: 12

MEDIUM: 25

LOW: 40
```

---

# Scan Specific Severity

## Critical Only

```bash
trivy image \
--severity CRITICAL \
nginx
```

---

## High and Critical

```bash
trivy image \
--severity HIGH,CRITICAL \
nginx
```

---

# Ignore Unfixed Vulnerabilities

```bash
trivy image \
--ignore-unfixed \
nginx
```

---

# Exit Code Configuration

Useful for CI/CD.

Fail Build:

```bash
trivy image \
--exit-code 1 \
--severity HIGH,CRITICAL \
myapp:v1
```

Build stops if vulnerabilities found.

---

# Filesystem Scanning

Scan Current Directory:

```bash
trivy fs .
```

---

# Scan Application Source

```bash
trivy fs /app
```

Scans:

```text
Dependencies

Packages

Secrets

Configuration Files
```

---

# Repository Scanning

```bash
trivy repo https://github.com/company/project
```

---

# Secret Scanning

Check Hardcoded Secrets

```bash
trivy fs \
--scanners secret .
```

Detects:

```text
Passwords

Tokens

SSH Keys

AWS Keys

API Credentials
```

---

# Terraform Scanning

## Terraform Security Scan

```bash
trivy config terraform/
```

---

# Example Issues

```text
Open Security Group

Public Access

Unencrypted Storage
```

---

# Kubernetes YAML Scan

```bash
trivy config deployment.yaml
```

---

# Example Findings

```text
Privileged Container

HostPath Usage

Missing Resources

Missing Security Context
```

---

# Kubernetes Cluster Scan

```bash
trivy kubernetes cluster
```

---

# Scan Namespace

```bash
trivy k8s \
--namespace production
```

---

# Dockerfile Security Scan

```bash
trivy config Dockerfile
```

Findings:

```text
Root User

Latest Tag Usage

Missing USER

Excessive Privileges
```

---

# SBOM Generation

SBOM:

```text
Software Bill of Materials
```

Generate SBOM:

```bash
trivy image \
--format cyclonedx \
-o sbom.json \
myapp:v1
```

---

# JSON Report

```bash
trivy image \
-f json \
-o report.json \
myapp:v1
```

---

# HTML Report

Generate JSON First:

```bash
trivy image \
-f json \
-o report.json \
myapp:v1
```

---

# Table Format

```bash
trivy image myapp:v1
```

Default Output:

```text
Table View
```

---

# Vulnerability Severity

## Critical

```text
Remote Exploit

System Compromise
```

---

## High

```text
Security Breach Risk
```

---

## Medium

```text
Moderate Risk
```

---

## Low

```text
Minimal Risk
```

---

# Docker Security Best Practices

## Use Official Images

Preferred:

```dockerfile
FROM nginx:stable
```

Avoid:

```dockerfile
FROM randomimage
```

---

## Use Non-Root User

```dockerfile
RUN useradd appuser

USER appuser
```

---

## Avoid Latest Tag

Bad:

```dockerfile
FROM nginx:latest
```

Good:

```dockerfile
FROM nginx:1.27
```

---

# Multi-Stage Build Example

```dockerfile
FROM maven AS build

RUN mvn package

FROM eclipse-temurin:17-jre

COPY app.jar app.jar
```

Benefits:

```text
Smaller Image

Fewer Vulnerabilities
```

---

# CI/CD Integration

## Jenkins Pipeline

```groovy
pipeline {

 agent any

 stages {

  stage('Trivy Scan') {

   steps {

    sh '''
       trivy image \
       --exit-code 1 \
       --severity HIGH,CRITICAL \
       myapp:v1
    '''

   }

  }

 }
}
```

---

# GitHub Actions Integration

```yaml
name: Trivy Scan

on:
  push:

jobs:

  scan:

    runs-on: ubuntu-latest

    steps:

    - uses: actions/checkout@v4

    - name: Build

      run: docker build -t myapp:v1 .

    - name: Scan

      run: |
        trivy image \
        --severity HIGH,CRITICAL \
        myapp:v1
```

---

# DevSecOps Workflow

```text
Developer

     |
     ▼

Git Commit

     |
     ▼

Build Image

     |
     ▼

Trivy Scan

     |
     ▼

Pass Security Gate

     |
     ▼

Docker Registry

     |
     ▼

Kubernetes
```

---

# Kubernetes Security Workflow

```text
Deployment YAML

        |
        ▼

Trivy Config Scan

        |
        ▼

Policy Validation

        |
        ▼

Kubernetes Cluster
```

---

# Common Trivy Commands

## Version

```bash
trivy version
```

---

## Image Scan

```bash
trivy image nginx
```

---

## Filesystem Scan

```bash
trivy fs .
```

---

## Secret Scan

```bash
trivy fs --scanners secret .
```

---

## Config Scan

```bash
trivy config .
```

---

## Kubernetes Scan

```bash
trivy kubernetes cluster
```

---

## Repo Scan

```bash
trivy repo https://github.com/project
```

---

# Daily DevSecOps Commands

```bash
trivy version

trivy image nginx

trivy image myapp:v1

trivy fs .

trivy config .

trivy fs --scanners secret .

trivy image \
--severity HIGH,CRITICAL \
myapp:v1

trivy image \
--ignore-unfixed \
myapp:v1
```

---

# Troubleshooting

## Database Update

```bash
trivy image nginx
```

Automatically updates vulnerability database.

---

## Debug Mode

```bash
trivy image \
--debug \
nginx
```

---

## Clear Cache

```bash
trivy clean --all
```

---

## Verify Internet Access

```bash
curl github.com
```

---

# Enterprise Architecture

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

Docker Build

      |
      ▼

Trivy Scan

      |
      ▼

Artifact Registry

      |
      ▼

Kubernetes

      |
      ▼

Production
```

---

# Production Security Flow

```text
Source Code
      |
      ▼

Trivy FS Scan

      |
      ▼

Docker Build

      |
      ▼

Trivy Image Scan

      |
      ▼

Registry

      |
      ▼

Kubernetes Scan

      |
      ▼

Production
```

---

# Trivy vs Traditional Security

Traditional:

```text
Scan After Deployment
```

Trivy:

```text
Shift Left Security

Scan Before Deployment
```

---

# Interview Questions

## What is Trivy?

```text
Open Source Vulnerability Scanner
```

---

## What Can Trivy Scan?

```text
Images

Kubernetes

Terraform

Repositories

Secrets

Filesystems
```

---

## Fail Build On Critical Vulnerability?

```bash
trivy image \
--exit-code 1 \
--severity CRITICAL \
myapp:v1
```

---

## Generate SBOM?

```bash
trivy image \
--format cyclonedx \
-o sbom.json \
myapp:v1
```

---

# Summary

✅ Trivy Installation

✅ Image Scanning

✅ Filesystem Scanning

✅ Secret Scanning

✅ Kubernetes Scanning

✅ Terraform Security

✅ Dockerfile Security

✅ SBOM Generation

✅ Jenkins Integration

✅ GitHub Actions Integration

✅ DevSecOps Security Gates

✅ Vulnerability Management

✅ Compliance Reporting

✅ Troubleshooting

✅ Enterprise Security Workflow

⭐ Keep this README as a complete Trivy Image Scanning and DevSecOps Security reference for DevOps Engineers, DevSecOps Engineers, SREs, Cloud Engineers, Security Engineers, and Platform Teams.

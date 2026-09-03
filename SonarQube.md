# SonarQube A to Z Guide
## Architecture, Configuration, Workflow & Administration

A complete SonarQube reference covering:

- SonarQube Fundamentals
- Architecture
- Installation
- Configuration
- Code Quality Analysis
- Static Application Security Testing (SAST)
- Quality Gates
- Quality Profiles
- CI/CD Integration
- Jenkins Integration
- GitHub Actions Integration
- Kubernetes Deployment
- DevSecOps Workflow
- Administration
- Troubleshooting
- Production Best Practices

---

# What is SonarQube?

SonarQube is a Code Quality and Security Analysis platform used to:

```text
Detect Bugs

Find Security Vulnerabilities

Identify Code Smells

Measure Technical Debt

Enforce Coding Standards

Improve Code Quality
```

---

# Why SonarQube?

Benefits:

```text
Static Code Analysis

Security Scanning

Quality Gate Enforcement

Code Coverage Tracking

Technical Debt Analysis

CI/CD Integration
```

---

# SonarQube Architecture

```text
Developer

    |
    ▼

Source Code

    |
    ▼

Sonar Scanner

    |
    ▼

SonarQube Server

    |
    ▼

Database

    |
    ▼

Dashboard & Reports
```

---

# Components

## SonarQube Server

Responsible For:

```text
Analyzing Reports

Quality Gates

Project Management

Dashboard Management
```

Default Port:

```text
9000
```

---

## Sonar Scanner

Responsible For:

```text
Sending Source Code Analysis
To SonarQube Server
```

---

## Database

Supported Databases:

```text
PostgreSQL (Recommended)

Oracle

SQL Server
```

Stores:

```text
Projects

Metrics

Users

Issues

Quality Profiles
```

---

# SonarQube Workflow

```text
Developer Commit

      |
      ▼

Git Repository

      |
      ▼

CI Pipeline

      |
      ▼

Sonar Scanner

      |
      ▼

SonarQube Server

      |
      ▼

Quality Gate

      |
      ▼

Pass / Fail Build
```

---

# Code Analysis Process

```text
Source Code

     |
     ▼

Scanner

     |
     ▼

Rules Engine

     |
     ▼

Issues Found

     |
     ▼

Dashboard
```

---

# Issue Categories

## Bugs

Example:

```java
int x = 10 / 0;
```

---

## Vulnerabilities

Example:

```java
SQL Injection

Cross Site Scripting (XSS)

Hardcoded Credentials
```

---

## Code Smells

Example:

```java
Unused Variables

Duplicate Code

Complex Methods
```

---

## Security Hotspots

Require Manual Review

Example:

```java
Encryption Logic

Authentication Logic
```

---

# SonarQube Metrics

## Reliability

```text
Bug Analysis
```

---

## Security

```text
Security Vulnerabilities
```

---

## Maintainability

```text
Code Smells
```

---

## Coverage

```text
Unit Test Coverage
```

---

## Duplication

```text
Duplicate Code
```

---

# SonarQube Installation Using Docker

## Create Container

```bash
docker run -d \
--name sonarqube \
-p 9000:9000 \
sonarqube:lts-community
```

---

# Verify Container

```bash
docker ps
```

---

# Access SonarQube

```text
http://SERVER-IP:9000
```

Default Login:

```text
Username : admin

Password : admin
```

---

# Docker Compose Setup

## docker-compose.yml

```yaml
services:

  sonarqube:

    image: sonarqube:lts-community

    container_name: sonarqube

    ports:
      - "9000:9000"

    restart: always
```

Start:

```bash
docker compose up -d
```

---

# Linux Installation

## Install Java

```bash
yum install java-17-openjdk -y
```

Verify:

```bash
java -version
```

---

# Download SonarQube

```bash
wget https://binaries.sonarsource.com/
```

Extract:

```bash
unzip sonarqube.zip
```

Start:

```bash
./sonar.sh start
```

---

# Verify Service

```bash
ps -ef | grep sonar
```

---

# Create Project

Navigate:

```text
Projects

    |
    ▼

Create Project
```

---

# Generate Token

```text
My Account

    |
    ▼

Security

    |
    ▼

Generate Token
```

Example:

```text
sonar-token-123
```

---

# Scanner Installation

## Download Scanner

```bash
wget sonar-scanner.zip
```

Extract:

```bash
unzip sonar-scanner.zip
```

Verify:

```bash
sonar-scanner --version
```

---

# Scanner Configuration

## sonar-project.properties

```properties
sonar.projectKey=calculator-app

sonar.projectName=Calculator App

sonar.host.url=http://localhost:9000

sonar.token=YOUR_TOKEN
```

---

# Run Analysis

```bash
sonar-scanner
```

---

# Maven Integration

## Analyze Java Maven Project

```bash
mvn clean verify sonar:sonar \
-Dsonar.token=TOKEN
```

---

# Gradle Integration

```bash
gradle sonarqube
```

---

# NodeJS Analysis

Install:

```bash
npm install
```

Run Scanner:

```bash
sonar-scanner
```

---

# Spring Boot Integration

```bash
mvn clean package

mvn sonar:sonar
```

---

# Quality Gates

Quality Gate determines whether:

```text
Build Passes

Build Fails
```

---

# Quality Gate Example

Conditions:

```text
Coverage > 80%

Critical Bugs = 0

Critical Vulnerabilities = 0
```

---

# Quality Profiles

Quality Profiles define:

```text
Coding Rules

Security Rules

Language Rules
```

---

# Supported Languages

```text
Java

Python

JavaScript

TypeScript

C#

C++

Go

PHP

Kotlin

Terraform
```

---

# Code Coverage

Integrate JaCoCo.

## Maven

```xml
<plugin>
  <groupId>org.jacoco</groupId>
</plugin>
```

Generate:

```bash
mvn test
```

Coverage displayed in SonarQube.

---

# CI/CD Integration

## Jenkins Workflow

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

Build

      |
      ▼

Sonar Analysis

      |
      ▼

Quality Gate

      |
      ▼

Deploy
```

---

# Jenkins Pipeline

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                sh 'mvn clean package'
            }
        }

        stage('Sonar Scan') {

            steps {

                sh '''
                mvn sonar:sonar \
                -Dsonar.token=TOKEN
                '''
            }
        }
    }
}
```

---

# GitHub Actions Integration

```yaml
name: SonarQube

on:
  push:

jobs:

  build:

    runs-on: ubuntu-latest

    steps:

    - uses: actions/checkout@v4

    - name: Build

      run: mvn clean package

    - name: Sonar Scan

      run: |
        mvn sonar:sonar \
        -Dsonar.token=${{ secrets.SONAR_TOKEN }}
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

Build

      |
      ▼

SonarQube Scan

      |
      ▼

Trivy Scan

      |
      ▼

Quality Gate

      |
      ▼

Deployment
```

---

# Kubernetes Deployment

## Deployment YAML

```yaml
apiVersion: apps/v1

kind: Deployment

metadata:
  name: sonarqube

spec:

  replicas: 1

  selector:
    matchLabels:
      app: sonarqube

  template:

    metadata:
      labels:
        app: sonarqube

    spec:

      containers:

      - name: sonarqube

        image: sonarqube:lts-community

        ports:
        - containerPort: 9000
```

Deploy:

```bash
kubectl apply -f sonarqube.yaml
```

---

# Security Best Practices

## Use PostgreSQL

Recommended Over Embedded DB.

---

## Enable Authentication

```text
LDAP

SAML

OAuth
```

---

## Use HTTPS

Configure Reverse Proxy:

```text
NGINX

Apache
```

---

## Backup Database

Regularly Backup:

```text
Projects

Rules

Reports

Users
```

---

# User Management

Navigate:

```text
Administration

      |
      ▼

Security

      |
      ▼

Users
```

Roles:

```text
Admin

Project Admin

User

Browse
```

---

# LDAP Integration

Example:

```properties
sonar.security.realm=LDAP
```

---

# Monitoring SonarQube

## Check Service

```bash
systemctl status sonarqube
```

---

## Process Check

```bash
ps -ef | grep sonar
```

---

## Port Check

```bash
ss -tulnp | grep 9000
```

---

# SonarQube Logs

Location:

```text
logs/
```

Files:

```text
sonar.log

web.log

ce.log

es.log
```

Monitor:

```bash
tail -f logs/sonar.log
```

---

# Common SonarQube Commands

## Start

```bash
./sonar.sh start
```

---

## Stop

```bash
./sonar.sh stop
```

---

## Restart

```bash
./sonar.sh restart
```

---

## Status

```bash
./sonar.sh status
```

---

# Troubleshooting

## Check Memory

```bash
free -m
```

---

## Check Java

```bash
java -version
```

---

## Check Logs

```bash
tail -f logs/sonar.log
```

---

## Verify Database

```bash
systemctl status postgresql
```

---

# Daily Administration Commands

```bash
sonar-scanner

mvn sonar:sonar

systemctl status sonarqube

ss -tulnp | grep 9000

docker ps

docker logs sonarqube

kubectl get pods

kubectl logs sonarqube-pod

tail -f logs/sonar.log
```

---

# Enterprise DevSecOps Architecture

```text
Developer

      |
      ▼

GitHub

      |
      ▼

Jenkins / GitHub Actions

      |
      ▼

Build

      |
      ▼

SonarQube

      |
      ▼

Quality Gate

      |
      ▼

Trivy

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

# SonarQube Maturity Model

```text
Level 1
Basic Code Analysis

Level 2
Quality Gates

Level 3
Coverage Tracking

Level 4
Security Analysis

Level 5
CI/CD Integration

Level 6
Enterprise DevSecOps Governance
```

---

# Interview Questions

## What is SonarQube?

```text
Static Code Quality
And Security Analysis Tool
```

---

## What is Quality Gate?

```text
Pass/Fail Criteria
For Code Quality
```

---

## Difference Between Bug and Code Smell?

```text
Bug = Functional Issue

Code Smell = Maintainability Issue
```

---

## What is Code Coverage?

```text
Percentage Of Code
Covered By Tests
```

---

## SonarQube Default Port?

```text
9000
```

---

# Summary

✅ SonarQube Fundamentals

✅ Architecture

✅ Installation

✅ Scanner Configuration

✅ Maven Integration

✅ Quality Gates

✅ Quality Profiles

✅ SAST Security Scanning

✅ Code Coverage

✅ Jenkins Integration

✅ GitHub Actions Integration

✅ Kubernetes Deployment

✅ DevSecOps Workflow

✅ User Administration

✅ Troubleshooting

✅ Production Best Practices

⭐ Keep this README as a complete SonarQube reference for DevOps Engineers, DevSecOps Engineers, Software Developers, SREs, Platform Engineers, Security Engineers, and Production Support Teams.

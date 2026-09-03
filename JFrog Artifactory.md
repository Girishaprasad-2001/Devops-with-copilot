# JFrog Artifactory A to Z Guide
## Architecture, Configuration, Workflow & Administration

A complete JFrog Artifactory reference covering:

- JFrog Platform Overview
- Artifactory Architecture
- Repository Management
- Binary Management
- Package Management
- Build Integration
- CI/CD Integration
- Security & Access Control
- Docker Registry
- Maven Repository
- NPM Repository
- Helm Repository
- Kubernetes Integration
- High Availability Setup
- Backup & Restore
- Monitoring
- Troubleshooting
- Production Best Practices

---

# What is JFrog?

JFrog is a DevOps Platform providing:

```text
Artifact Repository

Package Management

Container Registry

Build Management

Security Scanning

Release Management
```

---

# What is Artifactory?

Artifactory is JFrog's Universal Artifact Repository.

It stores:

```text
JAR Files

WAR Files

Docker Images

NPM Packages

Python Packages

Helm Charts

Terraform Modules

RPM Packages

Deb Packages
```

---

# Why Artifactory?

Benefits:

```text
Central Repository

Version Control

Artifact Traceability

Binary Storage

Secure Package Distribution

CI/CD Integration
```

---

# Artifactory Architecture

```text
Developer

     |
     ▼

Git Repository

     |
     ▼

Jenkins / GitHub Actions

     |
     ▼

Build Artifact

     |
     ▼

JFrog Artifactory

     |
     ▼

Deployment Environment

     |
     ▼

Production
```

---

# Enterprise Architecture

```text
                  Developers
                        |
                        ▼

                 CI/CD Pipeline
                        |
                        ▼

                JFrog Artifactory

         ┌──────────┼───────────┐

         ▼          ▼           ▼

      Maven       Docker      NPM

      Repo        Repo        Repo

         ▼          ▼           ▼

      Dev       Test       Production
```

---

# Artifact Lifecycle

```text
Source Code

     |
     ▼

Build

     |
     ▼

Artifact Generated

     |
     ▼

Store in Artifactory

     |
     ▼

Approval

     |
     ▼

Deploy

     |
     ▼

Production
```

---

# Artifactory Repository Types

---

## Local Repository

Stores internal artifacts.

Example:

```text
company-maven-release

company-docker-release
```

---

## Remote Repository

Proxies external repositories.

Examples:

```text
Maven Central

Docker Hub

NPM Registry
```

---

## Virtual Repository

Combines multiple repositories.

Example:

```text
maven-all
```

combining:

```text
local

remote

cache
```

---

# Repository Architecture

```text
Clients

   |
   ▼

Virtual Repository

   |
   ▼

Local Repositories

   |
   ▼

Remote Repositories
```

---

# Supported Package Types

```text
Maven

Gradle

NPM

Yarn

Docker

Helm

NuGet

PyPI

RPM

Debian
```

---

# Installation Methods

```text
Docker

Kubernetes

RPM

Deb Package

Binary Installation
```

---

# Docker Installation

## Run Artifactory

```bash
docker run -d \
--name artifactory \
-p 8081:8081 \
-p 8082:8082 \
jfrog/artifactory-oss
```

---

# Verify Container

```bash
docker ps
```

---

# Access Artifactory

```text
http://SERVER-IP:8082
```

---

# Docker Compose

## docker-compose.yml

```yaml
services:

  artifactory:

    image: jfrog/artifactory-oss

    container_name: artifactory

    ports:
      - "8081:8081"
      - "8082:8082"
```

Start:

```bash
docker compose up -d
```

---

# Artifactory Directory Structure

```text
artifactory/

├── data
├── logs
├── backup
├── etc
└── repositories
```

---

# Repository Creation

Navigate:

```text
Administration

    |
    ▼

Repositories

    |
    ▼

Local / Remote / Virtual
```

Create:

```text
Repository Key

Repository Type

Package Type
```

---

# Maven Repository Setup

## pom.xml

```xml
<distributionManagement>

  <repository>

    <id>artifactory</id>

    <url>
      http://server:8082/artifactory/libs-release-local
    </url>

  </repository>

</distributionManagement>
```

---

# Deploy Maven Artifact

```bash
mvn clean deploy
```

---

# Docker Repository Setup

Create Repository:

```text
docker-local
```

---

# Docker Login

```bash
docker login server:8082
```

---

# Build Image

```bash
docker build -t app:v1 .
```

---

# Tag Image

```bash
docker tag app:v1 \
server:8082/docker-local/app:v1
```

---

# Push Image

```bash
docker push \
server:8082/docker-local/app:v1
```

---

# Pull Image

```bash
docker pull \
server:8082/docker-local/app:v1
```

---

# NPM Repository Setup

Configure:

```bash
npm config set registry \
http://server:8082/artifactory/api/npm/npm-local
```

Publish:

```bash
npm publish
```

---

# Python Repository

Configure:

```bash
pip config set global.index-url \
http://server:8082/artifactory/api/pypi/pypi-local/simple
```

Upload:

```bash
twine upload dist/*
```

---

# Helm Repository

## Add Repository

```bash
helm repo add artifactory \
http://server:8082/artifactory/helm-local
```

Push Chart:

```bash
helm push app-chart.tgz artifactory
```

---

# User Management

Navigate:

```text
Administration

     |
     ▼

Identity

     |
     ▼

Users
```

---

# Create User

```text
Username

Email

Password

Groups
```

---

# Groups

Examples:

```text
Developers

DevOps

Release Managers

Administrators
```

---

# Permissions

Repository Permissions:

```text
Read

Write

Delete

Deploy

Annotate
```

---

# Access Control Flow

```text
User

   |
   ▼

Authentication

   |
   ▼

Authorization

   |
   ▼

Repository Access
```

---

# LDAP Integration

Navigate:

```text
Administration

     |
     ▼

Identity

     |
     ▼

LDAP
```

Example:

```text
Microsoft AD

OpenLDAP
```

---

# API Commands

## Get Version

```bash
curl \
-u admin:password \
http://server:8082/artifactory/api/system/version
```

---

# List Repositories

```bash
curl \
-u admin:password \
http://server:8082/artifactory/api/repositories
```

---

# Create Repository

```bash
curl -X PUT \
-u admin:password \
-H "Content-Type: application/json"
```

---

# JFrog CLI Installation

## Linux

```bash
curl -fL https://getcli.jfrog.io | sh
```

Verify:

```bash
jf --version
```

---

# Configure CLI

```bash
jf config add
```

---

# Ping Server

```bash
jf rt ping
```

---

# Upload Artifact

```bash
jf rt upload \
target/*.jar \
m*ven-local*
```

---

# Download Artifact

``*bash
jf rt download \
maven*local/app.jar
```

*--

# Search Artifacts

```bash*jf rt search "*.*ar"
```

*--

# Build Information

Collect B*ild:

```bash
jf rt build-publish *
my-build 1
```

*--

# CI/CD Integration

## Jenkin* Workflow

```text
GitHub

   |
*  ▼

J*nkins

   |
   ▼

Build

  *|
   ▼

JUnit

   |
   ▼*
*rtifact Upload

   |
  *▼

Artifactory

   |
   ▼*
Deployment
```

---

# Jenkins Pi*eline

```groovy
stage('Upload Art*fact') {

 steps {

  sh*'''
*   jf rt upload \
    target**.jar \
    maven-local/
  '''
 }
*
```

---

* GitHub Actions Integration

```ya*l
- name: Upload Artifact

  run: *
    jf rt upload \
    target/*.j*r \
    maven-local/
```

*--

# Kubernetes*Integration

Store:

```text
Helm *harts

Container Images

Deploymen* Packages
```

Workflow:

```text
*uild

   |
   ▼

*rtifactory

   |
*  ▼

Kubernetes*Pull* Image

*  |
   ▼

Deployment
```

*--

* Xray Integration

JFrog Xray prov*des:

```text*Vulnerability Scanning

License Co*pliance

Dependency Analysis
```

*--

#*Security Workflow

```text*Artifact Uploaded

      |
     *▼

Xray Scan

      |
      ▼*
*ulner*bilities Found

      |
      ▼

P*licy Check

      |
      ▼

Appro*e*/ Reject
```

*--

# Backup Strategy

Backup:

``*text
Repositories

Configuration

*atabase

Metadata
*``

---

* Backup Example

```bash*tar -czvf \
*rtifactory-backup.tar.gz \
/var**pt/jfrog/artifactory
```

*--

# High*Availability Architecture

```text*                Load Balancer

   *                  |
        ------*---------------------

        ▼  *                       ▼

 Artifac*ory Node1*       Artifactory Node2*
        ▼                        * ▼

         Shared Database

    *           |

                ▼

 *        Shared Storage
```

---

#*Monitoring

Monitor:

```text
CPU
*Memory

Storage

Repository Growth*
Download Metrics

Upload Metrics
*``

---

# Logs

Location*

*``text
logs/
```

*mportant Logs:

```text*art*factory.log

access.log

request*log

service.log
```

*--

* Troubleshooting Commands

## Chec* Port

```bash*ss*-tulnp | grep 8082
```

*--

## Docker*Logs

```bash*docker logs artifactory
```

*--

## Service Status

```bash*systemctl status artifactory
```

*--

## Check Storage

```bash
df -*
```

---

*# Monitor Logs*
```bash*tail -f artifactory.log
```

*--

# Daily Administration Command*

```bash
jf rt ping

jf rt upload*
*f rt download

jf rt search

docke* ps

docker logs artifactory

syst*mctl status artifactory

tail -f a*tifactory.log

df -h

ss -tulnp
``*

---

# Production Workflow

```t*xt
Developer

    |
    ▼

*it Commit

    |
    ▼*
CI/CD Pipeline

    |
    ▼*
Build Artifact

    |
    ▼*
Artifactory Upload

    |
    ▼*
Xray Security Scan

   *|
    ▼

Approval

    |
    ▼

De*loyment

    |
    ▼

Production
*``

---

# Best Practices

```text*Use Repository Separation

Use Vir*ual Repositories

Enable HTTPS

*ntegrate LDAP/*SO

*nable Backup Strategy

Use JFrog C*I

Enable Xray Scanning

*pply*RBAC

Monitor Repository Growth

*mplement*HA Architecture
``*

*--

# Artifactory vs Nexus

##*Art*factory

```text
Universal Reposit*ry

Strong Docker Support

Advance* Metadata

Xray Integration
```

#* Nexus

```text
Simpler Setup

Pop*lar Maven Usage

Lower Complexity
*``

---

# Interview Questions

##*What is Artifactory?

```text
Univ*rsal Artifact Repository
```

---
*## What are Repository Types?

```*ext
Local

Remote

Virtual
```

--*

## What is JFrog CLI?

```text
C*mmand Line Interface
For Managing *rtifacts
``*

*--

## What is Xray?

```text
Secu*ity And Vulnerability
Scanning Too*
```

---

# Summary

✅ JFrog Plat*orm

✅ Artifactory Architecture

✅*Repository Management

✅ Maven Rep*sitories

✅*Docker Registries

* NPM Repositories

✅ PyPI Reposito*ies

✅ Helm Repositories

✅*User*Management

✅ RBAC

✅ LDAP Integra*ion

✅ JFrog CLI

✅ Jenkins Integr*tion

✅ GitHub Actions Integration*
✅ Kubernetes Integration

✅ Xray *ecurity Scanning

✅ Backup & Resto*e

✅ High Availability

✅ Monitori*g & Troubleshooting

⭐ Keep this R*ADME as a complete JFrog Artifacto*y Administrator reference for DevO*s Engineers, DevSecOps Engineers* Release*Managers, Platform Engineers, SREs* Cloud Engineers, and Enterprise Administrators.

# Docker Multi-Stage Builds A to Z Guide
## Architecture, Configuration, Workflow & Administration

A complete Docker Multi-Stage Build reference covering:

- Docker Build Process
- Multi-Stage Build Architecture
- Build Optimization
- CI/CD Integration
- Security Best Practices
- Production Deployments
- Kubernetes Integration
- Troubleshooting
- Real-World Examples

---

# What is a Multi-Stage Build?

Multi-stage builds allow you to use multiple `FROM` statements in a single Dockerfile.

Benefits:

- Smaller Images
- Faster Deployments
- Improved Security
- Reduced Attack Surface
- Better Build Separation
- Easier CI/CD Pipelines

---

# Traditional Docker Build Problem

```text
Application Source
        |
        ▼
Build Tools
(Maven, JDK, Gradle)
        |
        ▼
Final Docker Image
```

Result:

```text
Large Image
Contains Build Tools
More Vulnerabilities
Higher Storage Usage
```

---

# Multi-Stage Build Solution

```text
Stage 1
-----------
Source Code
Build Tools
Compile Application

        |
        ▼

Stage 2
-----------
Only Application Artifact

        |
        ▼

Production Image
```

---

# Architecture Overview

```text
Source Code
      |
      ▼

Builder Stage
(Maven/JDK)

      |
      ▼

JAR/WAR File

      |
      ▼

Runtime Stage
(JRE Only)

      |
      ▼

Production Image
```

---

# Docker Build Workflow

```text
Developer
     |
     ▼

Git Repository

     |
     ▼

Docker Build

     |
     ▼

Stage 1
Compile Source

     |
     ▼

Stage 2
Copy Artifact

     |
     ▼

Final Image

     |
     ▼

Docker Registry

     |
     ▼

Kubernetes / Production
```

---

# Basic Multi-Stage Build

```dockerfile
# Stage 1

FROM maven:3.9.8-eclipse-temurin-17 AS builder

WORKDIR /app

COPY . .

RUN mvn clean package

# Stage 2

FROM eclipse-temurin:17-jre

WORKDIR /app

COPY --from=builder \
/app/target/app.jar app.jar

CMD ["java","-jar","app.jar"]
```

---

# How COPY --from Works

```dockerfile
COPY --from=builder \
/app/target/app.jar \
app.jar
```

Explanation:

```text
builder stage creates app.jar

runtime stage copies only app.jar

build dependencies are excluded
```

---

# Single Stage vs Multi-Stage

## Single Stage

```dockerfile
FROM maven:3.9.8-eclipse-temurin-17

COPY . .

RUN mvn clean package

CMD ["java","-jar","app.jar"]
```

Image Contains:

```text
JDK
Maven
Source Code
Dependencies
Build Cache
Application
```

---

## Multi-Stage

```dockerfile
FROM maven AS builder

FROM openjdk
```

Image Contains:

```text
Application Only
```

---

# Java Spring Boot Example

```dockerfile
FROM maven:3.9.8-eclipse-temurin-17 AS build

WORKDIR /app

COPY pom.xml .

COPY src ./src

RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jre

WORKDIR /app

COPY --from=build \
/app/target/*.jar app.jar

EXPOSE 8080

ENTRYPO*NT ["java","-jar","app.jar"]
```

*--

# NodeJS Multi-Stage Build

``*dockerfile*FROM node:22*AS build

WORKDIR /app

COPY packa*e*.json ./

RUN npm*install

COPY . .

RUN npm*run build

FROM nginx:alpine

COPY*--from=build \
/app/dist \
/usr*share/nginx/html
```

*--

# React*Application Example

```docker*ile*FROM node:22 AS builder

WORKDIR /*pp

COPY . .

RUN npm install*
RUN npm run build

FROM nginx*alpine

COPY --from=builder \
/*pp/build \
/usr/share/nginx/html
`*`

---

# Angular*Example

```*ocker*ile
FROM node:22 AS build

WORKDIR*/app

COPY . .

RUN npm install

R*N npm run build

FROM nginx

COPY *-from=build \
/app/dist/* \
/usr/s*are/nginx/html
```

---

# Python *ulti-Stage Build

```dockerfile
FR*M python:3.12 AS builder

WORKDIR *app

COPY requirements.txt .

RUN *ip install \
--prefix=/*nstall \
-r requirements.txt

FROM*python:*.12-slim

COPY --*rom=builder \
/install \
/usr/loca*

COPY . .

CMD ["python","app.py"]
```

---

# Golang Multi-Stage Bu*ld

```dockerfile
FROM golang:1.24*AS builder

WORKDIR /app

COPY . .*
RUN go*build -o app

FROM alpine

COPY --*rom=builder \
/app/app \
/app

CMD*["/app"]
```

*--

# Naming*Build Stages

```docker*ile
FROM maven AS builder

FROM ec*ipse-temurin AS runtime
```

Copy:*
```dockerfile
COPY --from=builder*...
```

---

# Build Specific Sta*e

```bash
docker build \
--target*build \
-t my-builder .
```

*seful for:

```text
Testing
Debugg*ng
Build Validation
```

*--

# Build Arguments

```*ockerfile*ARG VERSION=1.0

RUN echo*$VERSION
```

Build:

```bash*docker build \
--build-arg VERSION*2.0 .
```

---

# Environment Vari*bles

```dockerfile
ENV APP_ENV=pr*d
```

Verify:

```bash
docker ins*ect image_name
```

---

# Layer O*timization

Bad:

```docker*ile
RUN apt update

RUN apt instal* curl
```

Good:

```dockerfile*RUN apt update && \
    apt*install -y curl
```

*--

# Build Cache Optimization

``*dockerfile
COPY pom.xml .

RUN mv* dependency:go-offline

COPY src*./*rc
```

Benefits:

```text*Reuse Dependency Cache
*aster Builds
```

---

# Docker Ig*ore File

Create:

```text
.docker*gnore
```

Example:

```text
.git
*arget
node_modules
.idea
.vscode
`*`

Benefits:

```text
Smaller*Build Context
Faster Build
```

--*

# Security Best Practices

## Av*id Root User

```docker*ile
RUN useradd*appuser

USER appuser
```

---

##*Minimal Runtime Image

```dockerfi*e
FROM alpine
```

or

```dockerfi*e
FROM eclipse-temurin:17-jre
```
*---

## Never Store Secrets

Avoid*

```dockerfile
ENV PASSWORD=admin*23
```

*se:

```text
Vault
Secrets Manager*Kubernetes Secret
```

*--

# Multi*Stage Build with Non-*oot User

```dockerfile
FROM eclip*e-temurin:17-jre

RUN useradd appu*er

USER appuser

WORKDIR /app

CO*Y app.jar*.

ENTRYPOINT*["java","-jar","app.jar"]
``*

---

# Docker Build Commands

##*Build*Image

```bash*docker build -t myapp:v1 .
```

*--

## Build Without Cache

```bas**docker build --no-cache .
```

*--

## View Images

```bash*docker images
```

*--

## Inspect*Image

```bash*docker inspect myapp:v1
```

*--

# Multi*Architecture Build*
```bash*docker buildx build \
--platform*linux/amd64,linux/arm64 \
-t my*epo/app:latest .
```

*--

# Push Image

```bash*docker*push myrepo/app:v1
```

---

# Doc*er Registry Workflow

```text
Dock*r Build
       |
       ▼

Multi S*age Build

       |
       ▼

Opti*ized Image

       |
       ▼

Doc*er Registry

       |
       ▼

Ku*ernetes Deployment
```

*--

# Jenkins Integration*
```groovy
pipeline {

 agent any*
 stages {

  stage*'Build Image') {

   steps {

    *h 'docker build -t app:v1 .'

   }** }

  stage('Push Image') {

  *steps {

    sh*'docker push app:v1'

   }
  }
*}
*
*``

---

* GitHub Actions Example

```yaml*name: Docker Build

on:
  push*

*obs:

 build:

  runs*on: ubuntu-latest

  steps:

  -*uses: actions/checkout@v4

  -*name* Build

    run: docker build -t m*app .
```

---

# Kubernetes Deplo*ment

```yaml
apiVersion: apps/v1
*ind: Deployment

metadata:
  name:*springboot

spec:
* replicas: 2

  template*
    spec*
      containers:

      -*name* app

        image* myrepo/app:v1
```

*eploy:

```bash*kubectl apply -f deployment*yaml
```

*--

# Production Workflow

```text*Developer
      |
      ▼

Git Com*it

      |
      ▼

CI/CD Pipelin*

      |
      ▼*
Multi-Stage Build

      |
      *

Security Scan

      |
     *▼

Docker Registry

      |
      *

Kubernetes

      |
     *▼*
Production
```

*--

# Troubleshooting Commands

##*Build Logs

```bash
docker build .*```

---

## View Image Layers

``*bash
docker history myapp:v1
```

*--

## Inspect Image

```bash*docker inspect myapp:v1
```

*--

## Check Container

```bash*docker ps
```

*--

## Logs

```bash*docker logs container_id
```

*--

* Top Daily Docker Commands

```bas*
docker build .

docker build*--no-cache .

docker build*--target build .

docker images

d*cker ps

docker logs*
docker inspect

docker history

d*cker tag

docker push

docker pull*
*ocker run

docker exec*-it

docker stop

docker rm
``*

*--

# Interview Workflow

*``text
Source Code
      |
     *▼

Builder Image*
      |
      ▼

Compile Applicat*on

      |
      ▼

Artifact Gene*ated

      |
      ▼

Runtime*Image

     *|
      ▼

Copy Artifact

      |
*     ▼

Small Secure*Image

      |
      ▼

Deploy*```

---

* Benefits of Multi-Stage*Builds

✅ Smaller*Images

✅ Faster Deployments

✅ Be*ter Security

✅ Reduced Vulnerabil*ties

✅ Lower*Storage Costs

✅ Cleaner*Dockerfiles

✅ Better*CI/CD Pipelines

✅ Production*Ready Containers

---

* Summary

This guide covers:

- Do*ker*Multi-Stage Builds
- Builder*& Runtime Stages
- Java*Builds
- Spring Boot
- Node*S
- React
- Angular*- Python*- Golang
- Layer Optimization
-*Docker Caching
- Docker*Ignore
- Security Best Practices
-*Jenkins Integration
- GitHub Actio*s
- Kubernetes Deployment
- Produc*ion Workflow

⭐ Keep this README a* a complete Docker Multi-Stage Bui*d reference for Dev*ps Engineers, SREs, Platform Engin*ers, Cloud Engineers, and Producti*n Support Teams.
````*

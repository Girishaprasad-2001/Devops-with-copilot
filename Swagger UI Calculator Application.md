# Swagger UI Calculator Application
## Spring Boot + OpenAPI + Docker (Build & Run Example)

A complete hands-on project demonstrating:

- Spring Boot REST API
- Swagger UI Integration
- OpenAPI Documentation
- Maven Build
- Docker Multi-Stage Build
- Docker Container Deployment
- Kubernetes Deployment
- API Testing

---

# Project Overview

This application exposes Calculator APIs:

```text
Add
Subtract
Multiply
Divide
```

and automatically generates API documentation using:

```text
Swagger UI
OpenAPI 3
```

---

# Architecture

```text
+----------------+
| Swagger UI     |
+-------+--------+
        |
        ▼
+----------------+
| Spring Boot    |
| Calculator API |
+-------+--------+
        |
        ▼
+----------------+
| Business Logic |
+----------------+
```

---

# Request Flow

```text
User
  |
  ▼

Swagger UI

  |
  ▼

Spring Boot REST API

  |
  ▼

Calculator Service

  |
  ▼

Response Returned
```

---

# Project Structure

```text
calculator-api/

├── Dockerfile
├── pom.xml

└── src
    └── main
        ├── java
        │
        │   └── com/example/calculator
        │        ├── CalculatorApplication.java
        │        └── CalculatorController.java
        │
        └── resources
             └── application.yml
```

---

# Technology Stack

```text
Java 17
Spring Boot 3
Maven
OpenAPI 3
Swagger UI
Docker
Kubernetes
```

---

# Maven Configuration

## pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>

    <artifactId>calculator-api</artifactId>

    <version>1.0</version>

    <packaging>jar</packaging>

    <parent>
        <groupId>org.springframework.boot</groupId>

        <artifactId>
            spring-boot-starter-parent
        </artifactId>

        <version>3.5.0</version>
    </parent>

    <dependencies>

        <dependency>
            <groupId>
                org.springframework.boot
            </groupId>

            <artifactId>
                spring-boot-starter-web
            </artifactId>
        </dependency>

        <dependency>
            <groupId>org.springdoc</groupId>

            <artifactId>
              springdoc-openapi-starter-webmvc-ui
            </artifactId>

            <version>2.8.5</version>
        </dependency>

    </dependencies>

    <build>

        <plugins>

            <plugin>

                <groupId>
                    org.springframework.boot
                </groupId>

                <artifactId>
                    spring-boot-maven-plugin
                </artifactId>

            </plugin>

        </plugins>

    </build>

</project>
```

---

# Spring Boot Main Class

## CalculatorApplication.java

```java
package com.example.calculator;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CalculatorApplication {

    public static void main(String[] args) {

        SpringApplication.run(
                CalculatorApplication.class,
                args
        );

    }
}
```

---

# REST Controller

## CalculatorController.java

```java
package com.example.calculator;

import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.tags.Tag;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/calculator")
@Tag(name = "Calculator APIs")
public class CalculatorController {

    @GetMapping("/add")
    @Operation(summary = "Addition API")
    public int add(
            @RequestParam int a,
            @RequestParam int b) {

        return a + b;
    }

    @GetMapping("/subtract")
    @Operation(summary = "Subtraction API")
    public int subtract(
            @RequestParam int a,
            @RequestParam int b) {

        return a - b;
    }

    @GetMapping("/multiply")
    @Operation(summary = "Multiplication API")
    public int multiply(
            @RequestParam int a,
            @RequestParam int b) {

        return a * b;
    }

    @GetMapping("/divide")
    @Operation(summary = "Division API")
    public double divide(
            @RequestParam int a,
            @RequestParam int b) {

        if (b == 0) {
            throw new RuntimeException(
                "Division by zero not allowed"
            );
        }

        return (double) a / b;
    }
}
```

---

# Application Configuration

## application.yml

```yaml
server:
  port: 8080

springdoc:

  api-docs:
    enabled: true

  swagger-ui:
    enabled: true
```

---

# Build Application

## Compile Project

```bash
mvn clean package
```

Generated Artifact:

```text
target/calculator-api-1.0.jar
```

---

# Run Application

```bash
java -jar target/calculator-api-1.0.jar
```

Expected:

```text
Started CalculatorApplication
```

---

# Test APIs

## Addition

```bash
curl \
"http://localhost:8080/calculator/add?a=10&b=20"
```

Output:

```text
30
```

---

## Subtraction

```bash
curl \
"http://localhost:8080/calculator/subtract?a=20&b=10"
```

Output:

```text
10
```

---

## Multiplication

```bash
curl \
"http://localhost:8080/calculator/multiply?a=10&b=5"
```

Output:

```text
50
```

---

## Division

```bash
curl \
"http://localhost:8080/calculator/divide?a=100&b=5"
```

Output:

```text
20.0
```

---

# Swagger UI

Open Browser:

```text
http://localhost:8080/swagger-ui/index.html
```

---

# OpenAPI JSON

```text
http://localhost:8080/v3/api-docs
```

---

# Docker Multi-Stage Build

## Dockerfile

```dockerfile
# -------------------------
# Build Stage
# -------------------------

FROM maven:3.9.8-eclipse-temurin-17 AS builder

WORKDIR /app

COPY . .

RUN mvn clean package -DskipTests

# -------------------------
# Runtime Stage
# -------------------------

FROM eclipse-temurin:17-jre

WORKDIR /app

COPY --from=builder \
/app/target/calculator-api-1.0.jar \
app.jar

EXPOSE 8080

ENTRYPOINT ["java","-jar","app.jar"]
```

---

# Build Docker Image

```bash
docker build -t calculator-api:v1 .
```

Verify:

```bash
docker images
```

Expected:

```text
calculator-api:v1
```

---

# Run Docker Container

```bash
docker run -d \
--name calculator-api \
-p 8080:8080 \
calculator-api:v1
```

Verify:

```bash
docker ps
```

---

# View Logs

```bash
docker logs calculator-api
```

---

# Stop Container

```bash
docker stop calculator-api
```

---

# Remove Container

```bash
docker rm -f calculator-api
```

---

# Docker Compose

## docker-compose.yml

```yaml
services:

  calculator-api:

    image: calculator-api:v1

    container_name: calculator-api

    ports:
      - "8080:8080"

    restart: always
```

Start:

```bash
docker compose up -d
```

Verify:

```bash
docker compose ps
```

---

# Kubernetes Deployment

## deployment.yaml

```yaml
apiVersion: apps/v1

kind: Deployment

metadata:
  name: calculator-api

spec:

  replicas: 2

  selector:
    matchLabels:
      app: calculator-api

  template:

    metadata:
      labels:
        app: calculator-api

    spec:

      containers:

      - name: calculator-api

        image: calculator-api:v1

        ports:
        - containerPort: 8080
```

---

# Kubernetes Service

## service.yaml

```yaml
apiVersion: v1

kind: Service

metadata:
  name: calculator-api-service

spec:

  selector:
    app: calculator-api

  ports:
  - port: 8080
    targetPort: 8080

  type: NodePort
```

---

# Deploy to Kubernetes

```bash
kubectl apply -f deployment.yaml
```

```bash
kubectl apply -f service.yaml
```

Verify:

```bash
kubectl get pods
```

```bash
kubectl get svc
```

---

# CI/CD Workflow

```text
Developer
     |
     ▼
Git Commit
     |
     ▼
GitHub
     |
     ▼
GitHub Actions / Jenkins
     |
     ▼
Maven Build
     |
     ▼
Docker Image Build
     |
     ▼
Container Registry
     |
     ▼
Kubernetes Deployment
     |
     ▼
Swagger UI Validation
     |
     ▼
Production
```

---

# API Testing via Swagger

```text
Swagger UI
     |
     ▼
Try It Out
     |
     ▼
Send Request
     |
     ▼
View Response
     |
     ▼
Verify Result
```

---

# Useful Commands

## Maven

```bash
mvn clean
mvn package
mvn test
```

---

## Docker

```bash
docker build -t calculator-api:v1 .

docker run -p 8080:8080 calculator-api:v1

docker ps

docker logs calculator-api

docker stop calculator-api
```

---

## Kubernetes

```bash
kubectl get pods

kubectl get svc

kubectl describe pod

kubectl logs pod-name
```

---

# Troubleshooting

## Check Port

```bash
ss -tulnp | grep 8080
```

---

## Application Logs

```bash
tail -f application.log
```

---

## Container Logs

```bash
docker logs calculator-api
```

---

## Kubernetes Logs

```bash
kubectl logs <pod-name>
```

---

# End-to-End Workflow

```text
Developer
     |
     ▼
Spring Boot Code
     |
     ▼
OpenAPI Documentation
     |
     ▼
Swagger UI
     |
     ▼
Maven Package
     |
     ▼
Docker Multi-Stage Build
     |
     ▼
Docker Image
     |
     ▼
Container Runtime
     |
     ▼
Kubernetes Deployment
     |
     ▼
Production Usage
```

---

# Summary

This project demonstrates:

✅ Spring Boot REST API

✅ Swagger UI Integration

✅ OpenAPI 3 Documentation

✅ Calculator APIs

✅ Maven Build Process

✅ Docker Multi-Stage Build

✅ Docker Compose Deployment

✅ Kubernetes Deployment

✅ API Testing Using Swagger UI

✅ CI/CD Integration Ready

✅ Production-Ready Containerization

⭐ Use this project as a starter template for REST APIs with Swagger UI, OpenAPI, Docker, Kubernetes, and DevOps pipelines.

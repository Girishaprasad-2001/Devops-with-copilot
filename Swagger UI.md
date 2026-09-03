# Swagger UI / OpenAPI
# A to Z Guide: Architecture, Configuration, Workflow & Administration

A complete Swagger UI and OpenAPI reference covering:

- Swagger Fundamentals
- OpenAPI Specification
- Swagger UI Setup
- API Documentation
- REST API Workflow
- Configuration
- Spring Boot Integration
- NodeJS Integration
- Kubernetes Deployment
- API Security
- DevOps Integration
- API Testing
- Troubleshooting
- Production Best Practices

---

# What is Swagger?

Swagger is a framework for:

- API Documentation
- API Design
- API Testing
- API Development
- API Standardization

Today Swagger primarily works with:

```text
OpenAPI Specification (OAS)
```

Swagger UI provides a web interface to:

- View APIs
- Test APIs
- Explore API Documentation

---

# What is OpenAPI?

OpenAPI Specification (OAS) is a standard format used to describe REST APIs.

Example:

```yaml
openapi: 3.0.0

info:
  title: Employee API
  version: 1.0

paths:
  /employees:
    get:
      summary: Get Employees
```

---

# Swagger Architecture

```text
Developer
     |
     ▼

Application Code

     |
     ▼

OpenAPI Specification

     |
     ▼

Swagger UI

     |
     ▼

API Consumers
```

---

# API Documentation Workflow

```text
Developer

     |
     ▼

Create API

     |
     ▼

Add OpenAPI Annotations

     |
     ▼

Generate Documentation

     |
     ▼

Swagger UI

     |
     ▼

Consumer Uses API
```

---

# Why Swagger?

Benefits:

```text
Auto Documentation
Easy API Testing
Standardized APIs
Developer Friendly
Client SDK Generation
Reduced Manual Effort
```

---

# Swagger Components

## Swagger UI

Provides:

```text
Web Interface
API Testing
API Documentation
```

---

## OpenAPI Specification

Defines:

```text
Endpoints
Methods
Headers
Parameters
Responses
Schemas
Authentication
```

---

## Swagger Editor

Used To:

```text
Design APIs
Validate APIs
Edit OpenAPI Specs
```

---

## Swagger Codegen

Generates:

```text
Java Clients
Python Clients
NodeJS Clients
SDKs
```

---

# API Architecture

```text
Client

   |
   ▼

Swagger UI

   |
   ▼

REST API

   |
   ▼

Application

   |
   ▼

Database
```

---

# OpenAPI File Structure

```yaml
openapi: 3.0.0

info:
  title: Employee API
  version: 1.0

servers:
  - url: http://localhost:8080

paths:

components:

security:
```

---

# HTTP Methods

## GET

```http
GET /employees
```

Retrieve Data

---

## POST

```http
POST /employees
```

Create Data

---

## PUT

```http
PUT /employees/1
```

Update Data

---

## DELETE

```http
DELETE /employees/1
```

Delete Data

---

# Sample OpenAPI Specification

```yaml
openapi: 3.0.0

info:
  title: Employee API
  version: 1.0.0

paths:

  /employees:

    get:

      summary: Get Employees

      responses:

        '200':

          description: Success
```

---

# Spring Boot Integration

## Maven Dependency

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.8.5</version>
</dependency>
```

---

# Controller Example

```java
@RestController

@RequestMapping("/employees")

public class EmployeeController {

    @GetMapping

    public List<Employee> getEmployees() {

        return List.of();
    }

}
```

---

# Swagger URLs

```text
http://localhost:8080/swagger-ui.html
```

or

```text
http://localhost:8080/swagger-ui/index.html
```

---

# Spring Boot Workflow

```text
REST Controller

      |
      ▼

OpenAPI Scan

      |
      ▼

Swagger UI

      |
      ▼

API Documentation
```

---

# OpenAPI Annotations

## API Description

```java
@Tag(name="Employee APIs")
```

---

## Operation Description

```java
@Operation(summary="Get Employee")
```

---

## Response Documentation

```java
@ApiResponse(
responseCode = "200",
description = "Success"
)
```

---

# NodeJS Swagger Setup

## Install

```bash
npm install swagger-ui-express
```

```bash
npm install swagger-jsdoc
```

---

## Setup

```javascript
const swaggerUi =
require('swagger-ui-express');

app.use(
'/api-docs',
swaggerUi.serve,
swaggerUi.setup(swaggerSpec)
);
```

---

# Express API Documentation

```text
http://localhost:3000/api-docs
```

---

# API Request Flow

```text
User

 |
 ▼

Swagger UI

 |
 ▼

REST API

 |
 ▼

Application Logic

 |
 ▼

Database

 |
 ▼

Response

 |
 ▼

Swagger UI
```

---

# API Parameters

## Path Parameter

```http
GET /employees/{id}
```

---

## Query Parameter

```http
GET /employees?id=100
```

---

## Header Parameter

```http
Authorization: Bearer token
```

---

# Request Body Example

```json
{
  "name":"John",
  "salary":2000
}
```

---

# Response Example

```json
{
  "id":1,
  "name":"John"
}
```

---

# Authentication Types

## Basic Authentication

```yaml
securitySchemes:

  basicAuth:

    type: http

    scheme: basic
```

---

## JWT Authentication

```yaml
securitySchemes:

  bearerAuth:

    type: http

    scheme: bearer

    bearerFormat: JWT
```

---

# JWT Authorization Flow

```text
User Login
     |
     ▼

JWT Token

     |
     ▼

Swagger UI

     |
     ▼

Authorize Button

     |
     ▼

API Access
```

---

# Swagger Security Configuration

```yaml
security:
  - bearerAuth: []
```

---

# API Versioning

Version 1

```http
/api/v1/employees
```

Version 2

```http
/api/v2/employees
```

---

# Environment Configuration

## application.yml

```yaml
springdoc:

  api-docs:
    enabled: true

  swagger-ui:
    enabled: true
```

---

# Disable Swagger in Production

```yaml
springdoc:

  swagger-ui:
    enabled: false
```

---

# Docker Deployment

## Dockerfile

```dockerfile
FROM eclipse-temurin:17

COPY app.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
```

Build:

```bash
docker build -t swagger-app .
```

Run:

```bash
docker run -p 8080:8080 swagger-app
```

---

# Kubernetes Deployment

```yaml
apiVersion: apps/v1

kind: Deployment

metadata:
  name: swagger-app

spec:

  replicas: 2

  template:

    spec:

      containers:

      - name: app

        image: swagger-app:v1
```

Deploy:

```bash
kubectl apply -f deployment.yaml
```

---

# DevOps Workflow

```text
Developer

     |
     ▼

Code API

     |
     ▼

Generate OpenAPI Spec

     |
     ▼

Git Repository

     |
     ▼

CI/CD Pipeline

     |
     ▼

Docker Image

     |
     ▼

Kubernetes

     |
     ▼

Swagger UI
```

---

# CI/CD Integration

## Jenkins Pipeline

```groovy
stage('Build') {

 steps {

  sh 'mvn clean package'

 }

}
```

---

# GitHub Actions

```yaml
- name: Build

  run: mvn clean package
```

---

# API Testing Workflow

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

Validate API
```

---

# API Lifecycle

```text
Design

   |
   ▼

Develop

   |
   ▼

Document

   |
   ▼

Test

   |
   ▼

Deploy

   |
   ▼

Monitor
```

---

# Administration Activities

## Verify API Docs

```text
/swagger-ui
```

---

## Verify OpenAPI Spec

```text
/v3/api-docs
```

---

## Validate Specification

```yaml
openapi: 3.0.0
```

---

## Review Security

```text
JWT
OAuth2
Basic Auth
```

---

# Troubleshooting

## Swagger UI Not Loading

Check:

```bash
curl localhost:8080/swagger-ui
```

---

## Verify Application

```bash
systemctl status app
```

---

## Logs

```bash
journalctl -u application -f
```

---

## Spring Boot Logs

```bash
tail -f application.log
```

---

# Best Practices

## API Naming

Good:

```http
/employees
/orders
/customers
```

Avoid:

```http
/getEmployees
/deleteEmployee
```

---

## Response Codes

```text
200 Success
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
500 Internal Server Error
```

---

## Use Versioning

```http
/api/v1
/api/v2
```

---

## Secure Swagger

```text
Disable in Production
Enable Authentication
Restrict Access
```

---

# Enterprise Architecture

```text
Frontend

     |
     ▼

API Gateway

     |
     ▼

Microservices

     |
     ▼

Swagger/OpenAPI

     |
     ▼

Developer Portal
```

---

# Daily Commands

## Check Application

```bash
systemctl status app
```

## Docker

```bash
docker ps
```

## Kubernetes

```bash
kubectl get pods
```

## OpenAPI Endpoint

```bash
curl http://localhost:8080/v3/api-docs
```

---

# Interview Workflow

```text
REST API

     |
     ▼

Annotations

     |
     ▼

OpenAPI Document

     |
     ▼

Swagger UI

     |
     ▼

Testing

     |
     ▼

Consumer Integration
```

---

# Summary

This guide covers:

✅ Swagger UI

✅ OpenAPI Specification

✅ Spring Boot Integration

✅ NodeJS Integration

✅ API Documentation

✅ API Testing

✅ JWT Authentication

✅ Docker Deployment

✅ Kubernetes Deployment

✅ CI/CD Integration

✅ Security Best Practices

✅ DevOps Workflow

✅ API Administration

✅ Troubleshooting

✅ Production Configuration

⭐ Keep this README as a complete Swagger UI and OpenAPI reference for Developers, API Engineers, DevOps Engineers, Architects, SREs, and Production Support Teams.

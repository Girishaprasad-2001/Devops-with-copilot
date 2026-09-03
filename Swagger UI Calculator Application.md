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


# Swagger UI Calculator Application
## Spring Boot + OpenAPI + Docker (Build & Run Example)

This project demonstrates a complete REST API CRUD-style Calculator application using:

- Spring Boot 3
- OpenAPI 3
- Swagger UI
- Docker Multi-Stage Build
- Maven

The application exposes the following APIs:

```text
GET     - Retrieve calculations
POST    - Perform calculation
PUT     - Update calculation
DELETE  - Delete calculation
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
        │
        │      ├── CalculatorApplication.java
        │      ├── controller
        │      │     └── CalculatorController.java
        │      │
        │      ├── model
        │      │     └── Calculation.java
        │      │
        │      └── service
        │            └── CalculatorService.java
        │
        └── resources
             └── application.yml
```

---

# Maven Dependency

## pom.xml

```xml
<dependencies>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springdoc</groupId>
        <artifactId>
            springdoc-openapi-starter-webmvc-ui
        </artifactId>
        <version>2.8.5</version>
    </dependency>

</dependencies>
```

---

# Main Class

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

# Request Model

## Calculation.java

```java
package com.example.calculator.model;

public class Calculation {

    private int id;

    private double num1;

    private double num2;

    private String operation;

    private double result;

    public Calculation() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getNum1() {
        return num1;
    }

    public void setNum1(double num1) {
        this.num1 = num1;
    }

    public double getNum2() {
        return num2;
    }

    public void setNum2(double num2) {
        this.num2 = num2;
    }

    public String getOperation() {
        return operation;
    }

    public void setOperation(String operation) {
        this.operation = operation;
    }

    public double getResult() {
        return result;
    }

    public void setResult(double result) {
        this.result = result;
    }
}
```

---

# Service Layer

## CalculatorService.java

```java
package com.example.calculator.service;

import com.example.calculator.model.Calculation;

import org.springframework.stereotype.Service;

import java.util.*;

@Service
public class CalculatorService {

    private final Map<Integer, Calculation> db*=
            new HashMap<>();

  * public List<Calculation> findAll(* {
        return new ArrayList<>(*b.values());
    }

    public Cal*ulation findById(int id) {
       *return db.get(id);
    }

    publ*c Calculation save(Calculation cal*ulation) {

        double result *
                performOperation(*                        calculatio*.getNum1(),
                      * calculation.getNum2(),
          *             calculation.getOperat*on());

        calculation.setRes*lt(result);

        db.put(calcul*tion.getId(),
                calc*lation);

        return calculati*n;
    }

    public void delete(i*t id) {
        db.remove(id);
   *}

    private double performOpera*ion(
            double a,
       *    double b,
            String o*eration) {

        switch (operat*on.toLowerCase()) {

            c*se "add":
                return a*+ b;

            case "subtract":*                return a - b;

   *        case "multiply":
         *      return a * b;

            c*se "divide":
                retur* a / b;

            default:
    *           throw new RuntimeExcept*on(
                        "Inval*d Operation");
        }
    }
}
`*`

---

# REST Controller

## Calc*latorController.java

```java
pack*ge com.example.calculator.controll*r;

import com.example.calculator.*odel.Calculation;
import com.examp*e.calculator.service.CalculatorSer*ice;

import io.swagger.v3.oas.ann*tations.Operation;
import io.swagg*r.v3.oas.annotations.tags.Tag;

im*ort org.springframework.web.bind.a*notation.*;

import java.util.List*

@RestController
@RequestMapping(*/api/calculator")
@Tag(name = "Cal*ulator APIs")
public class Calcula*orController {

    private final *alculatorService service;

    pub*ic CalculatorController(
         *  CalculatorService service) {

  *     this.service = service;
    }*
    @GetMapping
    @Operation(su*mary =
            "Get All Calcul*tions")
    public List<Calculatio*> getAll() {

        return servi*e.findAll();
    }

    @GetMappin*("/{id}")
    @Operation(summary =*            "Get Calculation By Id*)
    public Calculation getById(
*           @PathVariable int id) {*
        return service.findById(i*);
    }

    @PostMapping
    @Op*ration(summary =
            "Crea*e Calculation")
    public Calcula*ion createCalculation(
           *@RequestBody Calculation request) *

        return service.save(requ*st);
    }

    @PutMapping("/{id}*)
    @Operation(summary =
       *    "Update Calculation")
    publ*c Calculation updateCalculation(
 *          @PathVariable int id,

 *          @RequestBody Calculation*request) {

        request.setId(*d);

        return service.save(r*quest);
    }

    @DeleteMapping(*/{id}")
    @Operation(summary =
 *          "Delete Calculation")
  * public String deleteCalculation(
*           @PathVariable int id) {*
        service.delete(id);

    *   return "Calculation Deleted";
 *  }
}
```

---

# Application Conf*guration

## application.yml

```y*ml
server:
  port: 8080

springdoc*

  swagger-ui:
    enabled: true
*  api-docs:
    enabled: true
```
*---

# API Documentation

## GET A*l Calculations

```http
GET /api/c*lculator
```

Response:

```json
[
  {
    "id": 1,
    "num1": 10,
    "num2": 20,
    "operation": "add",
    "result": 30
  }
]
```

---*
## GET By Id

```http
GET /api/ca*culator/1
```

Response:

```json
*
  "id": 1,
  "num1": 10,
  "num2"* 20,
  "operation": "add",
  "resu*t": 30
}
```

---

## POST Create *alculation

```http
POST /api/calc*lator
```

Request:

```json
{
  "*d": 1,
  "num1": 10,
  "num2": 20,*  "operation": "add"
}
```

Respon*e:

```json
{
  "id": 1,
  "num1":*10,
  "num2": 20,
  "operation": "*dd",
  "result": 30
}
```

---

##*PUT Update Calculation

```http
PU* /api/calculator/1
```

Request:

*``json
{
  "num1": 100,
  "num2": *0,
  "operation": "subtract"
}
```*
Response:

```json
{
  "id": 1,
 *"num1": 100,
  "num2": 20,
  "oper*tion": "subtract",
  "result": 80
*
```

---

## DELETE Calculation

*``http
DELETE /api/calculator/1
``*

Response:

```text
Calculation D*leted
```

---

# Build Project

`*`bash
mvn clean package
```

Gener*ted File:

```text
target/calculat*r-api.jar
```

---

# Run Applicat*on

```bash
java -jar target/calcu*ator-api.jar
```

---

# Swagger U*

Open:

```text
http://localhost:*080/swagger-ui/index.html
```

---*
# OpenAPI JSON

```text
http://lo*alhost:8080/v3/api-docs
```

---

* Dockerfile

## Multi-Stage Build
*```dockerfile
FROM maven:3.9.8-ecl*pse-temurin-17 AS builder

WORKDIR*/app

COPY . .

RUN mvn clean pack*ge -DskipTests

FROM eclipse-temur*n:17-jre

WORKDIR /app

COPY --fro*=builder \
/app/target/calculator-*pi.jar \
app.jar

EXPOSE 8080

ENT*YPOINT ["java","-jar","app.jar"]
`*`

---

# Build Docker Image

```b*sh
docker build -t calculator-api:*1 .
```

---

# Run Container

```*ash
docker run -d \
--name calcula*or-api \
-p 8080:8080 \
calculator*api:v1
```

---

# Verify Containe*

```bash
docker ps
```

---

# Ch*ck Logs

```bash
docker logs calcu*ator-api
```

---

# Test APIs Usi*g CURL

## POST

```bash
curl -X P*ST \
http://localhost:8080/api/cal*ulator \
-H "Content-Type: applica*ion/json" \
-d '{
"id":1,
"num1":1*,
"num2":20,
"operation":"add"
}'
*``

---

## GET

```bash
curl \
ht*p://localhost:8080/api/calculator
*``

---

## PUT

```bash
curl -X P*T \
http://localhost:8080/api/calc*lator/1 \
-H "Content-Type: applic*tion/json" \
-d '{
"num1":100,
"nu*2":20,
"operation":"subtract"
}'
`*`

---

## DELETE

```bash
curl -X*DELETE \
http://localhost:8080/api*calculator/1
```

---

# End-to-En* Workflow

```text
Developer
     *
     ▼
Spring Boot Code
     |
  *  ▼
Swagger/OpenAPI
     |
     ▼
*aven Build
     |
     ▼
Docker Im*ge
     |
     ▼
Docker Container
*    |
     ▼
Swagger UI
     |
   *

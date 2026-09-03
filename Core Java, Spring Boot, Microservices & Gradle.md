# Core Java, Spring Boot, Microservices & Gradle
## A to Z Guide: Architecture, Configuration, Workflow & Administration

A complete reference guide for Java Developers, Backend Engineers, Full-Stack Developers, Technical Leads, Architects, DevOps Engineers, and Production Support Teams.

---

# Table of Contents

1. Core Java Fundamentals
2. Object-Oriented Programming
3. Java Collections Framework
4. Exception Handling
5. Multithreading
6. Java 8+ Features
7. Spring Framework Overview
8. Spring Boot Architecture
9. Spring Boot Configuration
10. REST API Development
11. Microservices Architecture
12. Service Discovery
13. API Gateway
14. Configuration Management
15. Security
16. Database Integration
17. Gradle Build Tool
18. Logging & Monitoring
19. Dockerization
20. Kubernetes Deployment
21. CI/CD Workflow
22. Production Administration
23. Troubleshooting
24. Best Practices

---

# 1. Core Java Fundamentals

## Key Concepts

- JVM (Java Virtual Machine)
- JRE (Java Runtime Environment)
- JDK (Java Development Kit)
- Class Loader
- Memory Management
- Garbage Collection

### Verify Java Version

```bash
java -version
javac -version
```

---

# 2. Object-Oriented Programming (OOP)

## Four Pillars

### Encapsulation

```java
class Employee {
    private String name;

    public String getName() {
        return name;
    }
}
```

### Inheritance

```java
class Animal {}
class Dog extends Animal {}
```

### Polymorphism

```java
Animal obj = new Dog();
```

### Abstraction

```java
abstract class Vehicle {
    abstract void start();
}
```

---

# 3. Java Collections Framework

## List

```java
List<String> names = new ArrayList<>();
```

## Set

```java
Set<String> roles = new HashSet<>();
```

## Map

```java
Map<Integer,String> employees = new HashMap<>();
```

## Queue

```java
Queue<String> queue = new LinkedList<>();
```

---

# 4. Exception Handling

```java
try {
    int result = 10/0;
} catch(Exception ex){
    ex.printStackTrace();
} finally {
    System.out.println("Completed");
}
```

---

# 5. Multithreading

## Create Thread

```java
class Worker extends Thread {
    public void run() {
        System.out.println("Running");
    }
}
```

## Executor Service

```java
ExecutorService service =
Executors.newFixedThreadPool(5);
```

---

# 6. Java 8+ Features

## Lambda

```java
items.forEach(item -> System.out.println(item));
```

## Stream API

```java
numbers.stream()
       .filter(n -> n > 10)
       .forEach(System.out::println);
```

## Optional

```java
Optional<String> name =
Optional.of("Java");
```

---

# 7. Spring Framework Overview

## Core Modules

- Spring Core
- Spring MVC
- Spring Boot
- Spring Security
- Spring Data JPA
- Spring Cloud

---

# 8. Spring Boot Architecture

```text
            Client
              |
              ▼
          REST API
              |
              ▼
     Service Layer
              |
              ▼
    Repository Layer
              |
              ▼
          Database
```

---

# Spring Boot Project Structure

```text
src
 └── main
     ├── java
     │   ├── controller
     │   ├── service
     │   ├── repository
     │   ├── entity
     │   └── config
     │
     └── resources
         ├── application.yml
         └── bootstrap.yml
```

---

# 9. Spring Boot Configuration

## application.yml

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/appdb
    username: root
    password: admin

  jpa:
    hibernate:
      ddl-auto: update
```

---

## Profiles

```yaml
spring:
  profiles:
    active: dev
```

Run:

```bash
java -jar app.jar \
--spring.profiles.active=prod
```

---

# 10. REST API Development

## Controller

```java
@RestController
@RequestMapping("/employees")
public class EmployeeController {

    @GetMapping
    public List<Employee> getEmployees() {
        return employeeService.findAll();
    }
}
```

---

# CRUD APIs

```text
GET     /employees
GET     /employees/{id}
POST    /employees
PUT     /employees/{id}
DELETE  /employees/{id}
```

---

# 11. Microservices Architecture

## Traditional Monolith

```text
UI
 |
Application
 |
Database
```

---

## Microservices

```text
          API Gateway
                |
   -------------------------
   |           |           |
User Service Product Order
 Service      Service Service
   |           |           |
Database    Database    Database
```

---

# Benefits

- Independent Deployment
- Scalability
- Resilience
- Fault Isolation
- Technology Flexibility

---

# 12. Service Discovery

## Eureka Server

Dependency:

```gradle
implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-server'
```

Enable:

```java
@EnableEurekaServer
```

Client:

```java
@EnableDiscoveryClient
```

---

# 13. API Gateway

## Spring Cloud Gateway

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://USER-SERVICE
```

---

# 14. Centralized Configuration

## Spring Cloud Config

```text
Config Server
      |
Git Repository
      |
Microservices
```

Dependency:

```gradle
implementation 'org.springframework.cloud:spring-cloud-config-client'
```

---

# 15. Security

## Spring Security

Dependency:

```gradle
implementation 'org.springframework.boot:spring-boot-starter-security'
```

## JWT Authentication Flow

```text
User
 |
Login
 |
JWT Token
 |
API Gateway
 |
Microservices
```

---

# 16. Database Integration

## JPA Repository

```java
public interface EmployeeRepository
extends JpaRepository<Employee, Long> {
}
```

---

## Entity

```java
@Entity
public class Employee {

    @Id
    @GeneratedValue
    private Long id;
}
```

---

# 17. Gradle Build Tool

## Install

```bash
gradle -v
```

---

## build.gradle

```gradle
plugins {
    id 'java'
    id 'org.springframework.boot'
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

---

## Common Gradle Commands

### Build

```bash
gradle build
```

### Clean

```bash
gradle clean
```

### Run Tests

```bash
gradle test
```

### Skip Tests

```bash
gradle build -x test
```

### Dependency Tree

```bash
gradle dependencies
```

---

# 18. Logging & Monitoring

## Logback

```xml
<logger name="com.company"
level="INFO"/>
```

---

## Actuator

Dependency:

```gradle
implementation 'org.springframework.boot:spring-boot-starter-actuator'
```

Endpoints:

```text
/actuator/health
/actuator/info
/actuator/metrics
```

---

# 19. Dockerization

## Dockerfile

```dockerfile
FROM eclipse-temurin:21-jdk

COPY build/libs/app.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
```

Build:

```bash
docker build -t employee-service .
```

Run:

```bash
docker run -p 8080:8080 employee-service
```

---

# 20. Kubernetes Deployment

## deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: employee-service

spec:
  replicas: 3

  selector:
    matchLabels:
      app: employee-service

  template:
    metadata:
      labels:
        app: employee-service

    spec:
      containers:
      - name: employee-service
        image: employee-service:1.0
        ports:
        - containerPort: 8080
```

Deploy:

```bash
kubectl apply -f deployment.yaml
```

---

## Service

```yaml
apiVersion: v1
kind: Service

metadata:
  name: employee-service

spec:
  selector:
    app: employee-service

  ports:
    - port: 80
      targetPort: 8080

  type: ClusterIP
```

---

# 21. CI/CD Workflow

```text
Developer
      |
      ▼
 GitHub/GitLab
      |
      ▼
 Jenkins/Azure DevOps
      |
      ▼
 Gradle Build
      |
      ▼
 Unit Testing
      |
      ▼
 SonarQube Scan
      |
      ▼
 Docker Build
      |
      ▼
 Docker Registry
      |
      ▼
 Kubernetes Deployment
      |
      ▼
 Production
```

---

# 22. Production Administration

## JVM Monitoring

```bash
jps
jstack
jmap
jcmd
```

---

## Linux Monitoring

```bash
top
htop
free -m
df -h
netstat -tulpn
```

---

## Kubernetes Monitoring

```bash
kubectl top nodes
kubectl top pods
kubectl logs
kubectl describe pod
```

---

# 23. Troubleshooting Guide

## Application Not Starting

```bash
java -jar app.jar
```

Check:

```bash
journalctl -u app
```

---

## Port Already In Use

```bash
netstat -tulpn
```

or

```bash
lsof -i :8080
```

---

## Kubernetes CrashLoopBackOff

```bash
kubectl describe pod pod-name
```

```bash
kubectl logs pod-name
```

---

## OOM Error

```bash
kubectl top pod
```

Increase Memory Limits:

```yaml
resources:
  limits:
    memory: "1024Mi"
```

---

# 24. Best Practices

## Java

- Follow SOLID Principles
- Use Design Patterns
- Avoid Memory Leaks
- Use Streams Carefully

## Spring Boot

- Layered Architecture
- Externalized Configuration
- Use Profiles
- Enable Actuator

## Microservices

- Database per Service
- Stateless Services
- API Gateway
- Service Discovery

## Gradle

- Dependency Version Management
- Use Wrapper
- Build Cache

```bash
./gradlew build
```

---

# Architecture Overview

```text
                     Internet
                         |
                    Load Balancer
                         |
                     API Gateway
                         |
     ------------------------------------------------
     |                    |                        |
User Service      Employee Service       Order Service
     |                    |                        |
 PostgreSQL         PostgreSQL              PostgreSQL
     |
 Redis Cache
     |
 Kafka Messaging
     |
 Monitoring
(Prometheus/Grafana)
```

---

# Summary

This guide covers:

✅ Core Java

✅ OOP Concepts

✅ Collections Framework

✅ Multithreading

✅ Java 8 Features

✅ Spring Framework

✅ Spring Boot

✅ REST APIs

✅ Microservices

✅ Spring Cloud

✅ Eureka

✅ API Gateway

✅ Configuration Management

✅ Security & JWT

✅ Database Integration

✅ Gradle

✅ Docker

✅ Kubernetes

✅ CI/CD

✅ Monitoring

✅ Production Administration

✅ Troubleshooting

✅ Enterprise Architecture

⭐ Use this README as a complete reference for Java Full Stack, Spring Boot, Microservices, DevOps, Cloud, and Production Support environments.

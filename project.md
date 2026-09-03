# 1-Tier, 2-Tier & 3-Tier Architecture
# A to Z Guide: Architecture, Configuration, Workflow & Administration

A complete guide covering:

- 1-Tier Architecture
- 2-Tier Architecture
- 3-Tier Architecture
- Client-Server Communication
- Infrastructure Components
- Network Flow
- Deployment Models
- Security Considerations
- Administration
- Troubleshooting
- DevOps Integration
- Cloud Deployment Patterns

---

# Introduction

Application architectures define how system components communicate and where business logic, presentation, and data are placed.

The most common architectures are:

```text
1-Tier Architecture
2-Tier Architecture
3-Tier Architecture
```

---

# 1-Tier Architecture

## Definition

In 1-Tier Architecture:

```text
UI
Business Logic
Database

All Exist In Same System
```

Everything runs on a single machine.

---

# Architecture Diagram

```text
+------------------+
|   Application    |
|------------------|
| Presentation     |
| Business Logic   |
| Database         |
+------------------+
```

---

# Example

```text
MS Access
SQLite
Desktop Calculator
Single PC Application
```

---

# Workflow

```text
User
 |
 ▼

Application

 |
 ▼

Database

(All in Same Machine)
```

---

# Advantages

```text
Easy Setup
Low Cost
No Network Dependency
Simple Maintenance
```

---

# Disadvantages

```text
Limited Scalability
Single Point Of Failure
Poor Security
Not Suitable For Enterprise
```

---

# Use Cases

```text
Standalone Systems
Learning Environments
Personal Applications
Local Databases
```

---

# Example Setup

```text
Laptop

├── Frontend
├── Backend
└── SQLite Database
```

---

# 2-Tier Architecture

## Definition

In 2-Tier Architecture:

```text
Client
   |
   ▼
Database Server
```

Application directly communicates with the database.

---

# Architecture Diagram

```text
+--------------+
| Client App   |
+------+-------+
       |
       |
       ▼

+--------------+
| Database     |
| Server       |
+--------------+
```

---

# Workflow

```text
User

 |
 ▼

Client Application

 |
 ▼

SQL Query

 |
 ▼

Database Server

 |
 ▼

Response
```

---

# Examples

```text
Oracle Forms
MySQL Client Systems
Desktop ERP
Banking Client Applications
```

---

# Typical Technologies

## Client Layer

```text
Java Swing
.NET
Python Desktop Apps
Oracle Forms
```

## Database Layer

```text
MySQL
Oracle
PostgreSQL
SQL Server
```

---

# Advantages

```text
Simple Architecture
Better Performance
Easy Deployment
Suitable For Small Teams
```

---

# Disadvantages

```text
Database Exposed
Security Issues
Difficult Scaling
High DB Load
Client Dependency
```

---

# Example Deployment

```text
Client Machine
     |
     ▼

Database Server

MySQL Port:3306
PostgreSQL Port:5432
```

---

# 3-Tier Architecture

## Definition

3-Tier Architecture separates:

```text
Presentation Layer
Application Layer
Database Layer
```

This is the most widely used enterprise architecture.

---

# Architecture Diagram

```text
+------------------+
| Presentation     |
| Layer            |
| Browser/UI       |
+--------+---------+
         |
         ▼

+------------------+
| Application      |
| Layer            |
| Business Logic   |
+--------+---------+
         |
         ▼

+------------------+
| Database Layer   |
| MySQL/Postgres   |
+------------------+
```

---

# Detailed Architecture

```text
User
 |
 ▼

Browser

 |
 ▼

Web Server

 |
 ▼

Application Server

 |
 ▼

Database Server
```

---

# Enterprise Example

```text
Browser
    |
    ▼

Nginx

    |
    ▼

Spring Boot

    |
    ▼

MySQL
```

---

# Workflow

```text
Client Request
      |
      ▼

Web Server

      |
      ▼

Application Server

      |
      ▼

Database Query

      |
      ▼

Database

      |
      ▼

Response

      |
      ▼

Client
```

---

# Real-Time Example

## User Login

```text
User Login

      |
      ▼

Browser

      |
      ▼

Nginx

      |
      ▼

Spring Boot

      |
      ▼

MySQL User Table

      |
      ▼

Authentication Result

      |
      ▼

Browser
```

---

# Layer Breakdown

## Presentation Layer

Responsible For:

```text
User Interface
User Interaction
Input Validation
```

Examples:

```text
HTML
CSS
JavaScript
Angular
React
VueJS
```

---

## Application Layer

Responsible For:

```text
Business Logic
API Processing
Authentication
Authorization
```

Examples:

```text
Spring Boot
NodeJS
Python Flask
Django
.NET
```

---

## Database Layer

Responsible For:

```text
Data Storage
Transactions
Indexing
Backup
```

Examples:

```text
MySQL
PostgreSQL
Oracle
MongoDB
```

---

# Infrastructure Example

```text
Internet
    |
    ▼

Load Balancer

    |
    ▼

Nginx Servers

    |
    ▼

Application Servers

    |
    ▼

Database Servers
```

---

# Cloud 3-Tier Architecture

```text
Users

 |
 ▼

ALB (Load Balancer)

 |
 ▼

EC2 Web Servers

 |
 ▼

EC2 App Servers

 |
 ▼

RDS Database
```

---

# Kubernetes 3-Tier Architecture

```text
Ingress

   |
   ▼

Frontend Pods

   |
   ▼

Backend Pods

   |
   ▼

Database Pods
```

---

# Security Architecture

## 1-Tier

```text
Minimal Security
```

---

## 2-Tier

```text
Database Direct Exposure
```

---

## 3-Tier

```text
Network Segmentation
Firewall Rules
API Protection
RBAC
Better Security
```

---

# Scaling Comparison

## 1-Tier

```text
Vertical Scaling Only
```

---

## 2-Tier

```text
Limited Horizontal Scaling
```

---

## 3-Tier

```text
Horizontal Scaling
Load Balancing
Container Scaling
Auto Scaling
```

---

# Deployment Example

## Frontend Server

```text
NGINX
Apache
React
Angular
```

---

## Application Server

```text
Tomcat
Spring Boot
NodeJS
WebLogic
JBoss
```

---

## Database Server

```text
MySQL
PostgreSQL
Oracle
MongoDB
```

---

# DevOps Workflow

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

Docker

    |
    ▼

Kubernetes

    |
    ▼

Frontend

    |
    ▼

Backend

    |
    ▼

Database
```

---

# Monitoring Architecture

```text
Frontend

    |
    ▼

Node Exporter

    |
    ▼

Prometheus

    |
    ▼

Grafana
```

---

# Logging Architecture

```text
Application

     |
     ▼

Filebeat

     |
     ▼

Logstash

     |
     ▼

Elasticsearch

     |
     ▼

Kibana
```

---

# High Availability Architecture

```text
Users

 |
 ▼

Load Balancer

 |
 ▼

Web1      Web2

 |
 ▼

App1      App2

 |
 ▼

MySQL Primary

 |
 ▼

MySQL Replica
```

---

# Administration Tasks

## Web Layer

```bash
systemctl status nginx
systemctl restart nginx
```

---

## Application Layer

```bash
systemctl status tomcat
systemctl restart tomcat
```

---

## Database Layer

```bash
systemctl status mysqld
systemctl restart mysqld
```

---

# Troubleshooting Flow

```text
User Cannot Access Application

        |
        ▼

Check DNS

        |
        ▼

Check Load Balancer

        |
        ▼

Check Web Server

        |
        ▼

Check Application Server

        |
        ▼

Check Database

        |
        ▼

Resolve Issue
```

---

# Architecture Comparison

## 1-Tier

```text
Single System
No Network Dependency
Small Applications
```

---

## 2-Tier

```text
Client + Database
Small to Medium Applications
Direct DB Communication
```

---

## 3-Tier

```text
Presentation Layer

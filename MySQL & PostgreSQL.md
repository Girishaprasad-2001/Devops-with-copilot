# MySQL & PostgreSQL
# A to Z Guide: Architecture, Configuration, Workflow & Administration

A complete Database Administrator (DBA) reference covering:

- MySQL Architecture
- PostgreSQL Architecture
- Installation
- Configuration
- Database Administration
- User Management
- Backup & Restore
- Replication
- Performance Tuning
- Monitoring
- Security
- High Availability
- Kubernetes Deployment
- DevOps Integration
- Troubleshooting
- Production Best Practices

---

# Table of Contents

1. Introduction
2. MySQL Architecture
3. PostgreSQL Architecture
4. Installation
5. Configuration
6. User Management
7. Database Objects
8. Backup & Recovery
9. Replication
10. Performance Tuning
11. Monitoring
12. Security
13. Kubernetes Deployment
14. DevOps Workflow
15. Troubleshooting
16. Administration Commands

---

# What is MySQL?

MySQL is an open-source Relational Database Management System (RDBMS).

Popular Use Cases:

```text
Web Applications
E-Commerce
ERP Systems
CRM Systems
Microservices
```

Default Port:

```text
3306
```

---

# What is PostgreSQL?

PostgreSQL is an advanced open-source Object Relational Database Management System (ORDBMS).

Popular Use Cases:

```text
Enterprise Applications
Financial Systems
GIS Applications
Analytics Platforms
Large Scale Applications
```

Default Port:

```text
5432
```

---

# Database Architecture Overview

```text
Application
      |
      ▼

Database Server

      |
      ▼

Query Processor

      |
      ▼

Storage Engine

      |
      ▼

Disk Storage
```

---

# MySQL Architecture

```text
Client
  |
  ▼

MySQL Server

  |
  ├── Connection Manager
  ├── Query Parser
  ├── Optimizer
  ├── Executor
  └── Storage Engine

          |
          ▼

       InnoDB
```

---

# PostgreSQL Architecture

```text
Client

  |
  ▼

PostgreSQL Server

  |
  ├── Postmaster
  ├── Backend Process
  ├── WAL
  ├── Shared Buffers
  └── Query Planner

          |
          ▼

       Data Files
```

---

# Workflow

```text
Application
     |
     ▼

SQL Query

     |
     ▼

Database Engine

     |
     ▼

Query Execution

     |
     ▼

Results Returned
```

---

# MySQL Installation (RHEL/Rocky)

## Install

```bash
yum install mysql-server -y
```

## Start Service

```bash
systemctl enable mysqld

systemctl start mysqld
```

## Status

```bash
systemctl status mysqld
```

---

# PostgreSQL Installation

## Install

```bash
dnf install postgresql-server postgresql -y
```

---

## Initialize

```bash
postgresql-setup --initdb
```

---

## Start

```bash
systemctl enable postgresql

systemctl start postgresql
```

---

## Status

```bash
systemctl status postgresql
```

---

# MySQL Important Files

## Configuration

```bash
/etc/my.cnf
```

## Logs

```bash
/var/log/mysqld.log
```

## Data Directory

```bash
/var/lib/mysql
```

---

# PostgreSQL Important Files

## Configuration

```bash
/var/lib/pgsql/data/postgresql.conf
```

## Authentication

```bash
/var/lib/pgsql/data/pg_hba.conf
```

## Data Directory

```bash
/var/lib/pgsql/data
```

---

# MySQL Login

```bash
mysql -u root -p
```

---

# PostgreSQL Login

```bash
sudo -u postgres psql
```

---

# Database Commands

## Create Database

### MySQL

```sql
CREATE DATABASE companydb;
```

### PostgreSQL

```sql
CREATE DATABASE companydb;
```

---

# List Databases

### MySQL

```sql
SHOW DATABASES;
```

### PostgreSQL

```sql
\l
```

---

# Drop Database

```sql
DROP DATABASE companydb;
```

---

# User Administration

## Create User

### MySQL

```sql
CREATE USER 'devuser'@'%'
IDENTIFIED BY 'Password123';
```

---

### PostgreSQL

```sql
CREATE USER devuser
WITH PASSWORD 'Password123';
```

---

# Grant Privileges

### MySQL

```sql
GRANT ALL PRIVILEGES
ON companydb.*
TO 'devuser'@'%';
```

---

### PostgreSQL

```sql
GRANT ALL PRIVILEGES
ON DATABASE companydb
TO devuser;
```

---

# Revoke Access

### MySQL

```sql
REVOKE ALL PRIVILEGES
ON companydb.*
FROM 'devuser'@'%';
```

---

### PostgreSQL

```sql
REVOKE ALL PRIVILEGES
ON DATABASE companydb
FROM devuser;
```

---

# Table Commands

## Create Table

```sql
CREATE TABLE employees (

id INT PRIMARY KEY,

name VARCHAR(100),

salary DECIMAL(10,2)

);
```

---

## Insert Data

```sql
INSERT INTO employees
VALUES (1,'John',1000);
```

---

## Select Data

```sql
SELECT * FROM employees;
```

---

## Update Data

```sql
UPDATE employees
SET salary=2000
WHERE id=1;
```

---

## Delete Data

```sql
DELETE FROM employees
WHERE id=1;
```

---

# Backup Commands

## MySQL Backup

```bash
mysqldump \
-u root \
-p companydb \
> companydb.sql
```

---

## Restore

```bash
mysql \
-u root \
-p companydb \
< companydb.sql
```

---

# PostgreSQL Backup

```bash
pg_dump \
-U postgres \
companydb \
> companydb.sql
```

---

# Restore

```bash
psql \
-U postgres \
companydb \
< companydb.sql
```

---

# Service Commands

## MySQL

```bash
systemctl start mysqld

systemctl stop mysqld

systemctl restart mysqld

systemctl status mysqld
```

---

## PostgreSQL

```bash
systemctl start postgresql

systemctl stop postgresql

systemctl restart postgresql

systemctl status postgresql
```

---

# Replication Architecture

## MySQL Replication

```text
Primary Server
      |
      ▼

Binary Logs

      |
      ▼

Replica Server
```

---

## PostgreSQL Streaming Replication

```text
Primary

   |
   ▼

WAL Logs

   |
   ▼

Standby Server
```

---

# MySQL Replication Configuration

Edit:

```bash
/etc/my.cnf
```

```ini
server-id=1

log_bin=mysql-bin
```

Restart:

```bash
systemctl restart mysqld
```

---

# PostgreSQL Replication

Edit:

```bash
postgresql.conf
```

```ini
wal_level=replica

max_wal_senders=10
```

Edit:

```bash
pg_hba.conf
```

```ini
host replication all 0.0.0.0/0 md5
```

Restart:

```bash
systemctl restart postgresql
```

---

# Performance Tuning

## MySQL

```ini
innodb_buffer_pool_size=4G

max_connections=500
```

---

## PostgreSQL

```ini
shared_buffers=4GB

work_mem=64MB
```

---

# Monitoring Commands

## MySQL Processes

```sql
SHOW PROCESSLIST;
```

---

## PostgreSQL Sessions

```sql
SELECT * FROM pg_stat_activity;
```

---

# Database Size

### MySQL

```sql
SELECT
table_schema,
ROUND(SUM(data_length+index_length)
/1024/1024,2)
AS Size_MB
FROM information_schema.tables
GROUP BY table_schema;
```

---

### PostgreSQL

```sql
SELECT
pg_database.datname,
pg_size_pretty(
pg_database_size(pg_database.datname)
)
FROM pg_database;
```

---

# Security Best Practices

## Change Default Password

```sql
ALTER USER
```

---

## Restrict Remote Access

MySQL:

```ini
bind-address=127.0.0.1
```

PostgreSQL:

```ini
listen_addresses='localhost'
```

---

## Enable SSL

```text
TLS/SSL Certificates
```

---

## Principle of Least Privilege

Grant only required permissions.

---

# Kubernetes Deployment

## MySQL Deployment

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: mysql

spec:
  replicas: 1

  template:
    spec:
      containers:

      - name: mysql

        image: mysql:8.4

        env:
        - name: MYSQL_ROOT_PASSWORD
          value: password
```

Deploy:

```bash
kubectl apply -f mysql.yaml
```

---

# PostgreSQL Deployment

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: postgres

spec:
  replicas: 1

  template:
    spec:
      containers:
      - name: postgres
        image: postgres:17
```

Apply:

```bash
kubectl apply -f postgres.yaml
```

---

# DevOps Workflow

```text
Application
     |
     ▼

GitHub

     |
     ▼

CI/CD

     |
     ▼

Docker

     |
     ▼

Kubernetes

     |
     ▼

MySQL/PostgreSQL

     |
     ▼

Monitoring
```

---

# Docker Commands

## MySQL

```bash
docker run -d \
--name mysql \
-e MYSQL_ROOT_PASSWORD=password \
-p 3306:3306 \
mysql:8.4
```

---

## PostgreSQL

```bash
docker run -d \
--name postgres \
-e POSTGRES_PASSWORD=password \
-p 5432:5432 \
postgres:17
```

---

# Daily DBA Commands

## MySQL

```bash
mysql -u root -p

SHOW DATABASES;

SHOW PROCESSLIST;

SHOW VARIABLES;

SHOW STATUS;

SHOW TABLES;
```

---

## PostgreSQL

```bash
psql -U postgres

\l

\dt

\du

SELECT version();

SELECT * FROM pg_stat_activity;
```

---

# Troubleshooting Commands

## Port Verification

### MySQL

```bash
ss -tulnp | grep 3306
```

### PostgreSQL

```bash
ss -tulnp | grep 5432
```

---

## Logs

### MySQL

```bash
tail -f /var/log/mysqld.log
```

### PostgreSQL

```bash
tail -f /var/lib/pgsql/data/log/*
```

---

## Active Connections

### MySQL

```sql
SHOW PROCESSLIST;
```

### PostgreSQL

```sql
SELECT * FROM pg_stat_activity;
```

---

# Daily Administration Commands

```bash
systemctl status mysqld

systemctl restart mysqld

systemctl status postgresql

systemctl restart postgresql

mysql -u root -p

psql -U postgres

mysqldump

pg_dump

SHOW PROCESSLIST

SELECT * FROM pg_stat_activity

ss -tulnp

df -h

free -m
```

---

# Enterprise Database Architecture

```text
Application Servers
         |
         ▼

Load Balancer

         |
         ▼

Primary Database

         |
         ▼

Replication

         |
         ▼

Read Replica

         |
         ▼

Backup Storage
```

---

# MySQL vs PostgreSQL

## MySQL

```text
Faster Reads
Simple Administration
Popular for Web Applications
```

## PostgreSQL

```text
Advanced SQL Features
Better Complex Queries
Enterprise Grade Features
Strong ACID Compliance
```

---

# End-to-End Database Workflow

```text
Application
      |
      ▼
Connection
      |
      ▼
Authentication
      |
      ▼
Query Execution
      |
      ▼
Storage Engine
      |
      ▼
Database Files
      |
      ▼
Response Returned
```

---

# Summary

This guide covers:

✅ MySQL Architecture

✅ PostgreSQL Architecture

✅ Installation & Configuration

✅ Database Administration

✅ User Management

✅ Backup & Restore

✅ Replication

✅ Performance Tuning

✅ Monitoring

✅ Security

✅ Kubernetes Deployment

✅ Docker Setup

✅ DevOps Integration

✅ Troubleshooting

✅ Production Best Practices

⭐ Keep this README as a complete MySQL and PostgreSQL DBA reference for Database Administrators, DevOps Engineers, SREs, Cloud Engineers, and Production Support Teams.

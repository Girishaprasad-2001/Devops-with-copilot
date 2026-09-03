# Zabbix Administrator A to Z Guide
## Architecture, Configuration, Workflow & Administration

A complete Zabbix reference covering:

- Zabbix Architecture
- Installation
- Configuration
- Agent Setup
- Server Administration
- Monitoring Configuration
- Alerting
- Templates
- Discovery
- Auto Registration
- Linux Monitoring
- Windows Monitoring
- SNMP Monitoring
- Database Monitoring
- Kubernetes Monitoring
- Troubleshooting
- Production Best Practices

---

# What is Zabbix?

Zabbix is an Enterprise Open-Source Monitoring Solution used for:

- Infrastructure Monitoring
- Server Monitoring
- Network Monitoring
- Application Monitoring
- Cloud Monitoring
- Database Monitoring
- Container Monitoring
- Kubernetes Monitoring

---

# Zabbix Architecture

```text
                    +-----------------+
                    |     Users       |
                    +--------+--------+
                             |
                             ▼

                    +-----------------+
                    |     Zabbix UI   |
                    |     Frontend    |
                    +--------+--------+
                             |
                             ▼

                    +-----------------+
                    |  Zabbix Server  |
                    +--------+--------+
                             |
                    -------------------
                    |        |        |
                    ▼        ▼        ▼

             Zabbix   SNMP   IPMI

                    |
                    ▼

    +---------+  +---------+  +---------+
    | Linux   |  | Windows |  | Network |
    | Agent   |  | Agent   |  | Devices |
    +---------+  +---------+  +---------+

                    |
                    ▼

               Database
            (MySQL/PostgreSQL)
```

---

# Zabbix Components

## Zabbix Server

Responsible for:

- Collecting Data
- Trigger Evaluation
- Sending Alerts
- Data Processing

Default Port:

```text
10051
```

---

## Zabbix Agent

Installed on monitored servers.

Collects:

```text
CPU
Memory
Disk
Network
Processes
Services
Logs
```

Default Port:

```text
10050
```

---

## Zabbix Frontend

Provides:

```text
Dashboards
Monitoring
Configuration
Reports
Alert Management
```

Default Port:

```text
80
443
```

---

## Database

Supported:

```text
MySQL
MariaDB
PostgreSQL
Oracle
```

Stores:

```text
History
Events
Triggers
Hosts
Users
```

---

# Monitoring Workflow

```text
Linux Server
      |
      ▼
Zabbix Agent
      |
      ▼
Zabbix Server
      |
      ▼
Database
      |
      ▼
Zabbix Dashboard
      |
      ▼
Alerts & Reports
```

---

# Important Ports

```text
80 HTTP
443 HTTPS
10050 Agent
10051 Server
3306 MySQL
5432 PostgreSQL
```

---

# Step 1: Install Database

## MariaDB

```bash
yum install mariadb-server -y
```

Start:

```bash
systemctl enable mariadb
systemctl start mariadb
```

Verify:

```bash
systemctl status mariadb
```

---

# Create Database

```sql
CREATE DATABASE zabbix
CHARACTER SET utf8mb4
COLLATE utf8mb4_bin;
```

Create User:

```sql
CREATE USER 'zabbix'@'localhost'
IDENTIFIED BY 'Password123';
```

Grant Access:

```sql
GRANT ALL PRIVILEGES
ON zabbix.*
TO 'zabbix'@'localhost';
```

---

# Step 2: Install Zabbix Repository

RHEL/Rocky Linux Example

```bash
rpm -Uvh \
https://repo.zabbix.com/zabbix/7.0/rhel/9/x86_64/zabbix-release-latest.el9.noarch.rpm
```

Refresh Repository:

```bash
dnf clean all
```

---

# Step 3: Install Zabbix Server

```bash
dnf install zabbix-server-mysql \
zabbix-web-mysql \
zabbix-apache-conf \
zabbix-sql-scripts \
zabbix-agent -y
```

---

# Step 4: Import Database Schema

```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz \
| mysql -u zabbix -p zabbix
```

---

# Step 5: Configure Database Connection

Edit:

```bash
vi /etc/zabbix/zabbix_server.conf
```

Update:

```ini
DBName=zabbix

DBUser=zabbix

DBPassword=Password123
```

---

# Step 6: Start Services

```bash
systemctl enable zabbix-server
```

```bash
systemctl start zabbix-server
```

```bash
systemctl enable zabbix-agent
```

```bash
systemctl start zabbix-agent
```

```bash
systemctl restart httpd
```

---

# Verify

```bash
systemctl status zabbix-server
```

```bash
systemctl status zabbix-agent
```

---

# Step 7: Access Frontend

```text
http://SERVER-IP/zabbix
```

Default Login:

```text
Username : Admin
Password : zabbix
```

---

# Zabbix Agent Configuration

Edit:

```bash
vi /etc/zabbix/zabbix_agentd.conf
```

Modify:

```ini
Server=10.0.0.10

ServerActive=10.0.0.10

Hostname=webserver01
```

Restart:

```bash
systemctl restart zabbix-agent
```

---

# Test Agent Connectivity

From Server:

```bash
zabbix_get \
-s 10.0.0.20 \
-k system.hostname
```

Expected:

```text
webserver01
```

---

# Add Host

Navigate:

```text
Configuration
    |
    ▼
Hosts
    |
    ▼
Create Host
```

Add:

```text
Host Name
IP Address
Template
Host Group
```

Save.

---

# Templates

Templates provide prebuilt monitoring.

Examples:

```text
Linux by Zabbix Agent
Windows by Zabbix Agent
MySQL by Agent
Apache by Agent
NGINX by Agent
```

---

# Host Groups

Examples:

```text
Linux Servers
Production
Databases
Application Servers
Kubernetes
```

---

# Monitoring Linux Server

Apply Template:

```text
Linux by Zabbix Agent
```

Monitors:

```text
CPU
Memory
Disk
Processes
Load
Network
```

---

# Monitoring Windows Server

Install Agent.

Configure:

```ini
Server=10.0.0.10

Hostname=win-server01
```

Apply:

```text
Windows by Zabbix Agent
```

---

# Trigger Configuration

Example:

```text
CPU Usage > 90%
```

Expression:

```text
last(/webserver/system.cpu.util)>90
```

---

# Alerting Workflow

```text
Monitor Metric
       |
       ▼

Trigger Fires
       |
       ▼

Action Created
       |
       ▼

Notification Sent
       |
       ▼

Email/Teams/Slack
```

---

# Email Alert Configuration

Navigate:

```text
Administration
      |
      ▼
Media Types
      |
      ▼
Email
```

Configure SMTP:

```text
SMTP Server
Username
Password
Port
```

---

# SNMP Monitoring

Add Device:

```text
Router
Switch
Firewall
Load Balancer
```

Configure:

```text
SNMP Version
Community String
```

Example:

```text
public
```

---

# Discovery

Automatic Network Discovery.

Navigate:

```text
Configuration
    |
    ▼
Discovery
```

Create Rule:

```text
IP Range
SNMP
Agent
ICMP
```

---

# Auto Registration

Automatically add hosts.

Flow:

```text
New Server
     |
     ▼
Agent Installed
     |
     ▼
Auto Registration
     |
     ▼
Host Created
     |
     ▼
Template Applied
```

---

# Web Monitoring

Configure:

```text
Monitoring
     |
     ▼
Web Scenarios
```

Example:

```text
Check URL
Login Page
API Endpoint
```

---

# Database Monitoring

Supported:

```text
MySQL
PostgreSQL
Oracle
MongoDB
```

Apply Template:

```text
MySQL by Zabbix Agent
```

---

# Kubernetes Monitoring

Methods:

```text
Zabbix Agent
Prometheus Integration
Kubernetes API
```

Monitor:

```text
Nodes
Pods
Deployments
Namespaces
```

---

# Proxy Architecture

```text
Remote Site
      |
      ▼
Zabbix Proxy
      |
      ▼
Zabbix Server
```

Benefits:

```text
Reduced Traffic
Scalability
Remote Monitoring
```

---

# Dashboard Configuration

Navigate:

```text
Monitoring
     |
     ▼
Dashboards
```

Widgets:

```text
CPU
Memory
Problems
Maps
Top Hosts
Graphs
```

---

# User Management

Navigate:

```text
Administration
     |
     ▼
Users
```

Roles:

```text
Admin
User
Guest
Super Admin
```

---

# Backup Database

MySQL:

```bash
mysqldump -u root -p \
zabbix > zabbix_backup.sql
```

Restore:

```bash
mysql -u root -p zabbix < zabbix_backup.sql
```

---

# Daily Administration Commands

## Service Status

```bash
systemctl status zabbix-server
```

```bash
systemctl status zabbix-agent
```

---

## Restart Services

```bash
systemctl restart zabbix-server
```

```bash
systemctl restart zabbix-agent
```

---

## Agent Logs

```bash
tail -f /var/log/zabbix/zabbix_agentd.log
```

---

## Server Logs

```bash
tail -f /var/log/zabbix/zabbix_server.log
```

---

## Port Verification

```bash
ss -tulnp | grep 10050
```

```bash
ss -tulnp | grep 10051
```

---

# Troubleshooting Commands

## Verify Agent

```bash
zabbix_agentd -t system.hostname
```

## Check Connectivity

```bash
zabbix_get \
-s agent-ip \
-k system.hostname
```

## Service Logs

```bash
journalctl -u zabbix-server -f
```

```bash
journalctl -u zabbix-agent -f
```

---

# Top Daily Zabbix Commands

```bash
systemctl status zabbix-server

systemctl status zabbix-agent

systemctl restart zabbix-server

systemctl restart zabbix-agent

tail -f /var/log/zabbix/zabbix_agentd.log

tail -f /var/log/zabbix/zabbix_server.log

zabbix_get -s host -k system.hostname

ss -tulnp | grep 10050

ss -tulnp | grep 10051

mysql -u root -p zabbix
```

---

# Production Monitoring Workflow

```text
Linux Servers
      |
Windows Servers
      |
Network Devices
      |
Databases
      |
Applications
      |
      ▼

Zabbix Agents / SNMP

      |
      ▼

Zabbix Server

      |
      ▼

Database

      |
      ▼

Zabbix Frontend

      |
      ▼

Dashboards

      |
      ▼

Alerts

      |
      ▼

Email / Slack / Teams
```

---

# Enterprise Architecture

```text
                     Zabbix UI
                         |
                         ▼

                  Zabbix Server
                         |
            ------------------------
            |                      |
            ▼                      ▼

      Zabbix Proxy          Zabbix Proxy

            |                      |

            ▼                      ▼

      Linux Servers          Remote Sites

      Windows Servers        Branch Offices

      Databases             Network Devices
```

---

# Summary

This guide covers:

- Zabbix Installation
- Server Configuration
- Agent Configuration
- Host Management
- Templates
- Triggers
- Alerts
- Dashboards
- Discovery
- Auto Registration
- Proxy Setup
- Linux Monitoring
- Windows Monitoring
- SNMP Monitoring
- Database Monitoring
- Kubernetes Monitoring
- Backup & Restore
- Troubleshooting
- Production Administration

⭐ Keep this README as a complete Zabbix Administrator reference for DevOps Engineers, SREs, System Administrators, NOC Teams, Platform Engineers, and Production Support Teams.

# NGINX A to Z Commands, Architecture, Usage & Administration Cheat Sheet

A complete NGINX reference covering installation, configuration, reverse proxy, load balancing, SSL, monitoring, troubleshooting, and production support activities for DevOps Engineers, SREs, System Administrators, and Cloud Engineers.

---

# What is NGINX?

NGINX is a high-performance:

- Web Server
- Reverse Proxy
- Load Balancer
- API Gateway
- SSL Termination Server
- HTTP Cache Server

NGINX is commonly used in front of:

```text
Client
   |
   ▼
 NGINX
   |
   ├── Application Server
   ├── Tomcat
   ├── NodeJS
   ├── Spring Boot
   └── Kubernetes Ingress
```

---

# NGINX Request Flow (How It Works)

```text
User Request
      │
      ▼
 DNS Resolution
      │
      ▼
 NGINX Server
      │
      ▼
 Server Block Selection
      │
      ▼
 Location Block Match
      │
      ▼
 Reverse Proxy / Static Content
      │
      ▼
 Backend Server
      │
      ▼
 Response to User
```

---

# A. About NGINX

## Check Version

```bash
nginx -v
```

## Detailed Version

```bash
nginx -V
```

## Verify Installation

```bash
which nginx
```

---

# B. Basic Service Commands

## Start NGINX

```bash
systemctl start nginx
```

## Stop NGINX

```bash
systemctl stop nginx
```

## Restart NGINX

```bash
systemctl restart nginx
```

## Reload Configuration

```bash
systemctl reload nginx
```

## Check Status

```bash
systemctl status nginx
```

---

# C. Configuration Files

## Main Configuration

```bash
/etc/nginx/nginx.conf
```

## Site Configuration

Ubuntu/Debian:

```bash
/etc/nginx/sites-available/
```

Enabled Sites:

```bash
/etc/nginx/sites-enabled/
```

RedHat/CentOS:

```bash
/etc/nginx/conf.d/
```

---

# D. Display Configuration

## Show Entire Config

```bash
nginx -T
```

## Test Configuration

```bash
nginx -t
```

---

# E. Edit Configuration

```bash
vi /etc/nginx/nginx.conf
```

After changes:

```bash
nginx -t
systemctl reload nginx
```

---

# F. File Locations

## Web Root

```bash
/usr/share/nginx/html
```

## Default Index Page

```bash
index.html
```

---

# G. Generate SSL Certificate

## Self Signed SSL

```bash
openssl req -x509 -nodes -days 365 \
-newkey rsa:2048 \
-keyout nginx.key \
-out nginx.crt
```

---

# H. HTTP Configuration

## Basic HTTP Server

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
```

---

# I. Installation

## Ubuntu

```bash
sudo apt update
sudo apt install nginx -y
```

## RHEL/CentOS

```bash
sudo yum install nginx -y
```

---

# J. JSON Log Format

```nginx
log_format json escape=json
'{'
'"time":"$time_local",'
'"ip":"$remote_addr",'
'"status":"$status"'
'}';
```

---

# K. Kill Running Process

## Find Process

```bash
ps -ef | grep nginx
```

## Kill Process

```bash
kill -9 <PID>
```

---

# L. Logs

## Access Logs

```bash
tail -f /var/log/nginx/access.log
```

## Error Logs

```bash
tail -f /var/log/nginx/error.log
```

## Last 100 Lines

```bash
tail -100 /var/log/nginx/error.log
```

---

# M. Monitoring

## Open Connections

```bash
netstat -an | grep 80
```

## NGINX Processes

```bash
ps -ef | grep nginx
```

---

# N. Networking

## Check Port

```bash
ss -tulnp | grep nginx
```

or

```bash
netstat -tulnp | grep nginx
```

---

# O. Open Website

```text
http://localhost
```

```text
http://server-ip
```

---

# P. Process Control

## Reload Without Downtime

```bash
nginx -s reload
```

## Stop Gracefully

```bash
nginx -s quit
```

## Stop Immediately

```bash
nginx -s stop
```

---

# Q. Quick Verification

## Check Running Service

```bash
systemctl status nginx
```

## Verify Port

```bash
ss -tulnp | grep 80
```

## Verify Response

```bash
curl localhost
```

---

# R. Reverse Proxy

## NGINX → Tomcat

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://localhost:8080;
    }
}
```

---

# S. SSL Configuration

```nginx
server {
    listen 443 ssl;

    ssl_certificate /etc/nginx/nginx.crt;
    ssl_certificate_key /etc/nginx/nginx.key;
}
```

---

# T. Troubleshooting Commands

## Test Configuration

```bash
nginx -t
```

## View Error Logs

```bash
tail -f /var/log/nginx/error.log
```

## Verify Open Port

```bash
ss -tulnp
```

---

# U. Upstream Configuration

## Backend Pool

```nginx
upstream app_servers {
    server 10.1.1.10:8080;
    server 10.1.1.11:8080;
}
```

Usage:

```nginx
location / {
    proxy_pass http://app_servers;
}
```

---

# V. Virtual Hosting

## Multiple Domains

```nginx
server {
    server_name app1.example.com;
}

server {
    server_name app2.example.com;
}
```

---

# W. Web Server Usage

## Static Website Hosting

```nginx
server {
    root /var/www/html;
}
```

Deploy files:

```bash
cp index.html /var/www/html/
```

---

# X. Advanced Features

## Enable Compression

```nginx
gzip on;
```

## Enable Caching

```nginx
proxy_cache_path /tmp/cache levels=1:2 keys_zone=mycache:10m;
```

---

# Y. YAML Integration (CI/CD)

## Jenkins Example

```groovy
sh 'systemctl reload nginx'
```

## Ansible Example

```yaml
- name: Restart Nginx
  service:
    name: nginx
    state: restarted
```

---

# Z. Production Administration

## Backup Configuration

```bash
tar -czvf nginx-backup.tar.gz /etc/nginx
```

## Restore Configuration

```bash
tar -xzvf nginx-backup.tar.gz
```

---

# Reverse Proxy Step-by-Step Flow

## Step 1: Install NGINX

```bash
apt install nginx -y
```

## Step 2: Verify Installation

```bash
nginx -v
```

## Step 3: Configure Reverse Proxy

```nginx
server {

    listen 80;

    location / {
        proxy_pass http://localhost:8080;
    }
}
```

## Step 4: Test Config

```bash
nginx -t
```

## Step 5: Reload

```bash
systemctl reload nginx
```

## Step 6: Validate

```bash
curl localhost
```

Flow:

```text
Client
  │
  ▼
NGINX:80
  │
  ▼
Tomcat:8080
```

---

# Load Balancer Example

```nginx
upstream backend {

    server 10.0.0.1;
    server 10.0.0.2;
    server 10.0.0.3;
}

server {

    listen 80;

    location / {
        proxy_pass http://backend;
    }
}
```

Load balancing:

```text
Request 1 → Server1
Request 2 → Server2
Request 3 → Server3
Request 4 → Server1
```

---

# SSL Setup Workflow

```text
Generate Certificate
        │
        ▼
Configure SSL
        │
        ▼
Test Configuration
        │
        ▼
Reload NGINX
        │
        ▼
HTTPS Enabled
```

---

# Common NGINX Deployment Flow

```text
Install NGINX
      │
      ▼
Create Configuration
      │
      ▼
Test Configuration
      │
      ▼
Start Service
      │
      ▼
Configure SSL
      │
      ▼
Enable Reverse Proxy
      │
      ▼
Load Balancing
      │
      ▼
Production Traffic
      │
      ▼
Monitoring & Troubleshooting
```

---

# Top 25 NGINX Commands Used Daily

```bash
nginx -v
nginx -V
nginx -t
nginx -T

systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl reload nginx
systemctl status nginx

nginx -s reload
nginx -s stop
nginx -s quit

tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log

curl localhost

ps -ef | grep nginx

ss -tulnp | grep nginx
netstat -tulnp | grep nginx

lsof -i :80
lsof -i :443

df -h
free -m
top

cat /etc/nginx/nginx.conf
grep error /var/log/nginx/error.log
```

---

# NGINX Troubleshooting Commands

```bash
nginx -t

systemctl status nginx

journalctl -u nginx -f

tail -f /var/log/nginx/error.log

tail -f /var/log/nginx/access.log

curl localhost

ss -tulnp | grep 80

lsof -i :80

ps -ef | grep nginx

df -h

free -m

top
```

---

# NGINX Interview Flow (End-to-End)

```text
Browser Request
      │
      ▼
DNS Lookup
      │
      ▼
NGINX Listener (80/443)
      │
      ▼
Server Block
      │
      ▼
Location Block
      │
      ▼
Reverse Proxy
      │
      ▼
Application Server
      │
      ▼
Response
      │
      ▼
Client
```

---

# Summary

This cheat sheet covers:

- NGINX Installation
- Configuration Files
- Reverse Proxy
- Load Balancing
- SSL/TLS
- Virtual Hosts
- Caching
- Compression
- Logging
- Monitoring
- Performance Tuning
- Troubleshooting
- Jenkins Integration
- Ansible Automation
- Production Administration

⭐ Keep this README as a complete NGINX reference for DevOps, SRE, Cloud, Platform Engineering, and Production Support activities.

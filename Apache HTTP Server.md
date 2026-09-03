# Apache HTTP Server (HTTPD) A to Z Commands, Architecture, Usage & Administration Cheat Sheet

A complete Apache HTTP Server (HTTPD) reference covering installation, configuration, virtual hosts, reverse proxy, SSL, monitoring, troubleshooting, and production administration for DevOps Engineers, System Administrators, SREs, Middleware Engineers, and Cloud Engineers.

---

# What is HTTPD?

Apache HTTP Server (HTTPD) is an open-source web server used for:

- Hosting websites
- Reverse proxy
- Load balancing
- SSL/TLS termination
- URL rewriting
- Application serving

Typical flow:

```text
Client Browser
      │
      ▼
 Apache HTTPD
      │
 ┌────┼────┐
 ▼    ▼    ▼
PHP Tomcat NodeJS
```

---

# HTTPD Architecture

```text
User Request
      │
      ▼
DNS Resolution
      │
      ▼
Apache Listener (80/443)
      │
      ▼
Virtual Host Selection
      │
      ▼
Module Processing
      │
      ▼
Static Content / Proxy
      │
      ▼
Backend Application
      │
      ▼
Response
```

---

# A. About HTTPD

## Check Apache Version

```bash
httpd -v
```

Ubuntu:

```bash
apache2 -v
```

## Detailed Build Information

```bash
httpd -V
```

---

# B. Basic Service Commands

## Start HTTPD

```bash
systemctl start httpd
```

Ubuntu:

```bash
systemctl start apache2
```

## Stop HTTPD

```bash
systemctl stop httpd
```

## Restart HTTPD

```bash
systemctl restart httpd
```

## Reload Configuration

```bash
systemctl reload httpd
```

## Check Status

```bash
systemctl status httpd
```

---

# C. Configuration Files

## Main Configuration

RHEL/CentOS:

```bash
/etc/httpd/conf/httpd.conf
```

Ubuntu:

```bash
/etc/apache2/apache2.conf
```

## Virtual Hosts

```bash
/etc/httpd/conf.d/
```

Ubuntu:

```bash
/etc/apache2/sites-available/
```

---

# D. Display Configuration

## Show Loaded Modules

```bash
httpd -M
```

## Display Virtual Hosts

```bash
httpd -S
```

## Test Configuration

```bash
httpd -t
```

or

```bash
apachectl configtest
```

---

# E. Enable Modules

Ubuntu:

```bash
a2enmod rewrite
```

```bash
a2enmod ssl
```

Disable:

```bash
a2dismod rewrite
```

Restart:

```bash
systemctl restart apache2
```

---

# F. File Locations

## Web Root

```bash
/var/www/html
```

## Default Home Page

```bash
index.html
```

---

# G. Generate SSL Certificates

## Self-Signed Certificate

```bash
openssl req -x509 \
-newkey rsa:2048 \
-keyout server.key \
-out server.crt \
-days 365 \
-nodes
```

---

# H. HTTP Configuration

Example:

```apache
<VirtualHost *:80>
    ServerName example.com
  * DocumentRoot /var/www/html
</Virt*alHost>
```

---

# I. Install HTT*D

## RHEL/CentOS

```bash
yum ins*all httpd -y
```

## Rocky Linux

*``bash
dnf install httpd -y
```

*#*Ubuntu

```bash*apt install apache2 -y
```

---

#*J. Journal Logs

*# Service Logs

```bash
journalctl*-u httpd -f
``*

Ubuntu:

```bash*journalctl -u apache2 -f
```

*--

# K. Kill HTTPD Process

## Fi*d*Processes

```bash*ps -ef | grep httpd
```

##*Kill Process

```bash*kill -9*<PID>
```

---

# L. Logs

## Acce*s Logs

```bash
tail -f /var/log/h*tpd/access_log
```

Ubuntu:

```ba*h*tail -f /var/log/apache2/access.lo*
```

## Error Logs

```bash*tail -f /var*log/httpd/error_log
```

*buntu:

```bash*tail -f /var/log/apache2/error.log*```

*--

# M. Monitoring

## Running*Processes

```bash*ps*-ef | grep httpd
```

*#*Resource Utilization

```bash
top
*``

---

# N. Network*Commands

## Check Listening Ports*
```bash
*s -tulnp | grep httpd
*``

or

```bash*net*tat -tulnp | grep httpd
```

---

* O. Open Website

```*ext
http://localhost
```

*``text
http://server-ip
```

---

* P. Process Control

##*Graceful Reload

```bash*apachectl graceful
```

*# Restart*
```bash
apachectl restart
```

*# Stop*
```bash
apachectl stop
```

*--

# Q. Quick Verification

## Ve*ify*Service

```bash*systemctl status httpd
```

## Ver*fy Port

```bash*ss -tulnp | grep 80
``*

## Test Website

```bash*curl*http://localhost
```

*--

# R.*Reverse Proxy

## Proxy to Tomcat
*```apache
ProxyPass / http://localhost:8080/
ProxyPassReverse / http://localhost:8080/
```

Enable modul*s:

```bash
a2enmod proxy
a2enmod *roxy_http
```

---

# S. SSL Confi*uration

```apache
<VirtualHost *:*43>

 SSLEngine on

 SS*Certificate*ile /etc/ssl/server.crt

 SSLC*rtificateKeyFile /etc/ssl/server.k*y

</Virtual*ost>
```

---

# T. Troubleshootin* Commands

*# Test Configuration

```bash
http* -t
```

## View Logs

```bash*tail -f /var/log/httpd/error_log
`*`

---

#*U. URL Rewriting

Enable rewrite m*dule:

```bash
a2enmod rewrite
```*
Example:

```apache
RewriteEngine*On

RewriteRule ^old$ /new [R=301,*]
```

---

# V. Virtual Hosts

##*Multiple Websites

```apache
<Virt*alHost *:80>
 ServerName app1.exam*le.com
 DocumentRoot /var/www/app1*</VirtualHost>

<VirtualHost *:80>* ServerName app2.example.com
 Docu*entRoot /var/www/app2
</VirtualHos*>
```

---

# W. Web Hosting

## S*atic Website

```apache
DocumentRo*t "/var/www/html"
```

Deploy site*

```bash
cp index.html /var/www/h*ml/
```

---

# X. Extra Administr*tion

## List Open Files

```bash
*sof -p <PID>
```

## Validate Conf*g

```bash
apachectl configtest
``*

---

# Y. YAML Integration (Auto*ation)

## Ansible Example

```yam*
- name: Restart HTTPD
  service:
*   name: httpd
    state: restarte*
```

## Jenkins Example

```groov*
sh 'systemctl restart httpd'
```
*---

# Z. Advanced Administration
*## Backup Configuration

```bash
t*r -czvf httpd-backup.tar.gz /etc/h*tpd
```

## Restore Configuration
*```bash
tar -xzvf httpd-backup.tar*gz
```

---

# Virtual Host Setup *low

## Step 1: Create Website Dir*ctory

```bash
mkdir -p /var/www/a*p1
```

## Step 2: Create Index Pa*e

```bash
echo "Welcome to App1" * /var/www/app1/index.html
```

## Step 3: Configure Virtual Host

```apache
<VirtualHost *:80>
 ServerName app1.example.com
 DocumentRoot /var/www/app1
</VirtualHost>
```

## Step 4: Validate Configuration

```bash
httpd -t
```

## Step 5: Restart Service

```bash
systemctl restart httpd
```

## Step 6: Verify Access

```bash
curl http://app1.example.com
```

---

# Reverse Proxy Flow

```text
Browser
   │
   ▼
Apache HTTPD :80
   │
   ▼
Proxy Module
   │
   ▼
Tomcat :8080
   │
   ▼
Application Response
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
Enable SSL Module
        │
        ▼
Restart Apache
        │
        ▼
HTTPS Access
```

---

# Common Deployment Workflow

```text
Install HTTPD
      │
      ▼
Configure Virtual Hosts
      │
      ▼
Deploy Website
      │
      ▼
Enable SSL
      │
      ▼
Restart Service
      │
      ▼
Verify Website
      │
      ▼
Monitor Logs
```

---

# Top 25 HTTPD Commands Used Daily

```bash
httpd -v
httpd -V

httpd -M
httpd -S

httpd -t
apachectl configtest

systemctl start httpd
systemctl stop httpd
systemctl restart httpd
systemctl reload httpd
systemctl status httpd

apachectl graceful
apachectl restart

ps -ef | grep httpd

ss -tulnp | grep httpd
netstat -tulnp | grep httpd

tail -f /var/log/httpd/access_log
tail -f /var/log/httpd/error_log

curl localhost

lsof -p <PID>

df -h
free -m
top

journalctl -u httpd -f
```

---

# HTTPD Troubleshooting Commands

```bash
systemctl status httpd

httpd -t

apachectl configtest

tail -f /var/log/httpd/error_log

tail -f /var/log/httpd/access_log

journalctl -u httpd -f

ss -tulnp | grep 80

curl localhost

ps -ef | grep httpd

df -h

free -m

top
```

---

# HTTPD Interview End-to-End Flow

```text
Browser Request
      │
      ▼
DNS Lookup
      │
      ▼
Apache Listener (80/443)
      │
      ▼
Virtual Host Selection
      │
      ▼
Rewrite Rules
      │
      ▼
Proxy or Static Content
      │
      ▼
Backend Application
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

- HTTPD Installation
- Configuration Files
- Virtual Hosts
- Reverse Proxy
- SSL/TLS
- URL Rewriting
- Web Hosting
- Logging
- Monitoring
- Performance Tuning
- Troubleshooting
- Jenkins Integration
- Ansible Automation
- Production Administration
- Middleware Support

⭐ Keep this README as a complete Apache HTTPD quick reference for DevOps, SRE, Platform Engineering, Cloud Infrastructure, and Production Support activities.

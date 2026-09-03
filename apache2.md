# Apache2 (Apache HTTP Server) A to Z Commands, Architecture, Usage & Administration Cheat Sheet

A complete Apache2 reference covering installation, configuration, virtual hosting, reverse proxy, SSL, monitoring, troubleshooting, security, and production administration for DevOps Engineers, SREs, Cloud Engineers, Middleware Administrators, and Linux System Administrators.

---

# What is Apache2?

Apache2 is an open-source web server used for:

- Website Hosting
- Reverse Proxy
- SSL/TLS Termination
- URL Rewriting
- Load Balancing
- Application Hosting
- API Gateway

---

# Apache2 Architecture

```text
Client Browser
      │
      ▼
DNS Resolution
      │
      ▼
Apache2 Listener (80/443)
      │
      ▼
Virtual Host Selection
      │
      ▼
Modules Processing
      │
 ┌────┼───────┐
 ▼    ▼       ▼
PHP Proxy  Static Content
      │
      ▼
Backend Application
      │
      ▼
Response
```

---

# A. Apache2 Information

## Check Version

```bash
apache2 -v
```

## Detailed Build Information

```bash
apache2 -V
```

## Show Loaded Modules

```bash
apache2ctl -M
```

---

# B. Basic Service Commands

## Start Apache2

```bash
systemctl start apache2
```

## Stop Apache2

```bash
systemctl stop apache2
```

## Restart Apache2

```bash
systemctl restart apache2
```

## Reload Apache2

```bash
systemctl reload apache2
```

## Service Status

```bash
systemctl status apache2
```

---

# C. Configuration Locations

## Main Configuration File

```bash
/etc/apache2/apache2.conf
```

## Ports Configuration

```bash
/etc/apache2/ports.conf
```

## Sites Available

```bash
/etc/apache2/sites-available/
```

## Sites Enabled

```bash
/etc/apache2/sites-enabled/
```

## Modules Available

```bash
/etc/apache2/mods-available/
```

## Modules Enabled

```bash
/etc/apache2/mods-enabled/
```

---

# D. Display Configuration

## Test Configuration

```bash
apache2ctl configtest
```

or

```bash
apache2ctl -t
```

## Show Virtual Hosts

```bash
apache2ctl -S
```

## Dump Running Configuration

```bash
apache2ctl -M
```

---

# E. Enable Modules

## Enable Rewrite Module

```bash
a2enmod rewrite
```

## Enable SSL Module

```bash
a2enmod ssl
```

## Enable Proxy Module

```bash
a2enmod proxy
```

## Enable HTTP Proxy Module

```bash
a2enmod proxy_http
```

Reload:

```bash
systemctl reload apache2
```

---

# F. File Management

## Default Web Root

```bash
/var/www/html
```

## List Hosted Files

```bash
ls -ltr /var/www/html
```

## Deploy Application

```bash
cp index.html /var/www/html/
```

---

# G. Generate SSL Certificates

## Create Self-Signed Certificate

```bash
openssl req -x509 \
-nodes \
-days 365 \
-newkey rsa:2048 \
-keyout server.key \
-out server.crt
```

---

# H. HTTP Configuration

## Basic Virtual Host

```apache
<VirtualHost *:80>

    ServerName example.com

*   Document*oot /var/www/html*
</Virtual*ost>
```

---

# I. Installation

*# Ubuntu/Debian

```bash*sudo apt update*sudo apt install apache2 -y
``*

## Verify Installation

```*ash
apache2 -v
```

---

# J. Jour*al*Logs

## Follow Service Logs

```b*sh
journalctl*-u apache2 -f
``*

## Recent Logs

```bash*journalctl -u apache2 -n 100*```

---

# K. Kill Apache*Process

## Find*Process

```bash*ps -ef | grep apache*
```

*# Kill Process

```bash*kill -9 <PID>
```

---

# L. Logs
*##*Access Logs

```bash*tail -f /*ar/log/apache2/access.log
```

## *rror Logs

```bash
tail -f /var/lo*/apache2/error.log
```

## Search *rrors

```bash
grep ERROR /var/log*apache2/error.log
```

---

# M. M*nitoring

## View Running Processe*

```bash
ps -ef | grep apache2
``*

## Monitor System Usage

```bash*top
```

## Memory Usage

```bash
*ree -m
```

---

# N. Network Comm*nds

## Check Listening Ports

```*ash
ss -tulnp | grep apache2
*``

or

```bash*netstat -tulnp | grep apache2
```
*---

# O. Open Website

```text**ttp://localhost
```

```text
http:*/server-ip
```

---

# P. Process *ontrol

## Graceful Restart

```ba*h
apachectl graceful
```

## Resta*t

```bash*apachectl restart
```

## Stop

``*bash
apachectl stop
```

---

# Q.*Quick Verification

## Verify Serv*ce

```bash
systemctl status apach*2
```

## Verify Port

```bash
ss *tulnp | grep 80
```

## Verify Web*ite

```bash
curl http://localhost*```

---

# R. Reverse Proxy Confi*uration

## Apache2 to Tomcat

```*pache
ProxyPass / http://localhost*8080/
ProxyPassReverse / http://lo*alhost:8080/
```

Enable modules:
*```bash
a2enmod proxy
a2enmod prox*_http
```

---

# S. SSL Configura*ion*
*``apache
<VirtualHost *:443>

 SSL*ngine on

 SSLCertificateFile /etc*ssl/server.crt

 SSLCertificateKey*ile /etc/ssl/server.key

</Virtual*ost>
```

---

# T. Troubleshootin* Commands

## Validate Config

```*ash
apache2ctl configtest
```

## *heck Logs

```bash
tail -f /var/lo*/apache2/error.log
```

---

# U. *RL Rewriting

Enable rewrite:

```*ash
a2enmod rewrite
```

Example:
*```apache
RewriteEngine On

Rewrit*Rule ^old$ /new [R=301,L]
```

*--

# V. Virtual Hosts

## Multipl* Domains

```*pache
*VirtualHost *:80>

 ServerName app*.example.com

 DocumentRoot /var/w*w/app1

</VirtualHost>

<VirtualHo*t *:80>

 ServerName app2.example.*om

 DocumentRoot /var/www/app2

<*Virtual*ost>
```

---

# W. Web Hosting

#* Static Website Hosting

```*pache
DocumentRoot "/var/www/html"*```

Deploy:

```bash
cp website/**/var/www/html/
```

---

# X. Extr* Administration

## List Open File*

```bash
lsof -p <PID>
```

## Op*n Connections

```bash
netstat -an*| grep 80
```

---

# Y. YAML Inte*ration

## Ansible Example

```yam*
- name: Restart Apache2
  service*
    name: apache2
    state: rest*rted
```

## Jenkins Example

```g*oovy
sh 'systemctl restart apache2*
```

---

# Z. Advanced Administr*tion

## Backup Configuration

```*ash
tar -czvf apache2-backup.tar.g* /etc/apache2
```

## Restore Conf*guration

```bash
tar -xzvf apache*-backup.tar.gz
```

---

# Virtual*Host Setup Step-by-Step

## Step 1* Create Website Directory

```bash*mkdir -p /var/www/app1
```

## Ste* 2: Create Website

```bash
echo "*elcome to App1" > /var/www/app1/in*ex.html
```

## Step 3: Create Vir*ual Host

```bash
vi /etc/apache2/*ites-available/app1.conf
```

Add:*
```apache
<VirtualHost *:80>

 Se*verName app1.example.com

 Documen*Root /var/www/app1

</VirtualHost>*```

## Step 4: Enable Site

```ba*h
a2ensite app1.conf
```

## Step *: Test Configuration

```bash
apac*e2ctl configtest
```

## Step 6: R*start Service

```bash
systemctl r*start apache2
```

## Step 7: Veri*y

```bash
curl http://app1.exampl*.com
```

---

# Reverse Proxy Flo*

```text
Client Request
      │
 *    ▼
Apache2 :80
      │
      ▼
*roxy Module
      │
      ▼
Tomcat*8080
      │
      ▼
Spring Boot A*plication
      │
      ▼
Response*```

---

# SSL Setup Workflow

``*text
Generate SSL Certificate
    *     │
          ▼
Enable SSL Modu*e
          │
          ▼
Configur* Virtual Host
          │
        * ▼
Test Configuration
          │
*         ▼
Restart Apache2
       *  │
          ▼
HTTPS Enabled
```
*---

# Common Apache2 Deployment W*rkflow

```text
Install Apache2
  *     │
        ▼
Configure Website*        │
        ▼
Create Virtual*Host
        │
        ▼
Enable Si*e
        │
        ▼
Test Configu*ation
        │
        ▼
Restart *pache2
        │
        ▼
Verify *ebsite
        │
        ▼
Enable *SL
        │
        ▼
Production *raffic
```

---

# Top 25 Apache2 *ommands Used Daily

```bash
apache* -v
apache2 -V

apache2ctl -M
apac*e2ctl -S

apache2ctl configtest

s*stemctl start apache2
systemctl st*p apache2
systemctl restart apache*
systemctl reload apache2
systemct* status apache2

apachectl gracefu*
apachectl restart

a2enmod rewrit*
a2enmod ssl
a2enmod proxy

a2ensi*e app.conf
a2dissite app.conf

tai* -f /var/log/apache2/access.log
ta*l -f /var/log/apache2/error.log

j*urnalctl -u apache2 -f

curl local*ost

ss -tulnp | grep 80
netstat -*ulnp | grep 80

ps -ef | grep apac*e2
```

---

# Apache2 Troubleshoo*ing Commands

```bash
systemctl st*tus apache2

apache2ctl configtest*
journalctl -u apache2 -f

tail -f*/var/log/apache2/error.log

tail -* /var/log/apache2/access.log

curl*localhost

ss -tulnp | grep 80

ls*f -i :80

ps -ef | grep apache2

f*ee -m

df -h

top
```

---

# Apac*e2 Interview End-to-End Flow

```t*xt
Browser Request
      │
      ▼*DNS Resolution
      │
      ▼
Apa*he2 Listener
      │
      ▼
Virtu*l Host Match
      │
      ▼
Modul* Processing
      │
      ▼
Static*Content / Reverse Proxy
      │
  *   ▼
Application Server
      │
  *   ▼
Response Returned
      │
   *  ▼
Browser
```

---

# Summary

T*is cheat sheet covers:

- Apache2 *nstallation
- Service Management
-*Virtual Hosting
- Reverse Proxy
- *SL/TLS
- URL Rewriting
- Logging &*

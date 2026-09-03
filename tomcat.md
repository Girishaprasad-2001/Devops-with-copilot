# Apache Tomcat A to Z Commands & Administration Cheat Sheet

A complete Apache Tomcat reference covering installation, configuration, deployment, administration, monitoring, troubleshooting, security, and production operations for DevOps Engineers, Middleware Administrators, SREs, and Java Developers.

---

# A. About Tomcat

## Check Java Version

```bash
java -version
```

## Check Tomcat Version

```bash
catalina.sh version
```

or

```bash
./version.sh
```

---

# B. Basic Directory Structure

```text
TOMCAT_HOME/
├── bin/
├── conf/
├── lib/
├── logs/
├── temp/
├── webapps/
└── work/
```

| Directory | Purpose |
|------------|------------|
| bin | Startup and shutdown scripts |
| conf | Configuration files |
| logs | Application and server logs |
| webapps | WAR deployments |
| lib | Shared libraries |
| temp | Temporary files |
| work | JSP compiled files |

---

# C. Configure Environment Variables

## Set JAVA_HOME

```bash
export JAVA_HOME=/usr/java/jdk17
```

## Verify JAVA_HOME

```bash
echo $JAVA_HOME
```

## Set CATALINA_HOME

```bash
export CATALINA_HOME=/opt/tomcat
```

---

# D. Download Tomcat

## Download Package

```bash
wget https://downloads.apache.org/tomcat/
```

## Extract Archive

```bash
tar -xvzf apache-tomcat-10.x.tar.gz
```

---

# E. Enable Startup Scripts

```bash
chmod +x *.sh
```

```bash
chmod +x*$CATALINA_HOME/bin**.sh
```

---

# F. File*Management

## List Web Applicatio*s

```bash*ls -ltr*webapps/
```

*# Deploy*WAR

```bash*cp application.war $CATALINA_HOME/*ebapps/
```

---

# G. Generate Lo*s*
*# View*Cataline Logs

```bash*tail -f logs*catalina.out
*``

## View Recent Logs

```bash*tail -100 logs/catalina.out
```

-*-

# H. HTTP Connector*Configuration*
Edit:

```bash
conf/server.xml
``*

Default port:

```xml*<Connector port="8080"
protocol="*TTP/1.1"/>
*``

---

# I. Install*Tomcat

## Extract*Package

```bash*tar -zx*f apache-tomcat.tar.gz
```

*# Move Installation

```bash*mv*apache-tomcat /opt/tomcat
*``

*--

# J. Java Configuration

## Ch*ck Java Process

```bash
ps -ef | *rep java
```

*# Verify*Java Home

```bash
echo $JAVA_HOME*```

---

# K. Kill*Tom*at Process

```bash*ps -ef | grep tomcat
``*

```*ash
kill -9 <PID*
```

---

# L. Logs*Management

## Catalina*Log

```bash*logs*catalina.out
```

*# Localhost Log

```bash*logs/localhost.log
```

*# Follow Log

*``bash*tail -f logs/catalina.out
```

*--

# M. Memory Configuration

## *et*Heap Memory

```bash**xport CATALINA_OPTS="-Xms*G -Xmx4G*
```

*# Verify JVM Parameters

```*ash
ps -ef | grep java
```

*--

# N. Network Commands*
## Check Port

```*ash*netstat -tulnp*| grep 8080
```

or

```bash*ss -t*lnp | grep 8080
*``

---

# O. Open Tom*at

## Browser Access

```text*http://localhost:8080
```

*--

# P.*Process Monitoring

## Check*Running Process

```bash*ps -ef | grep tom*at
```

## CPU Usage

```bash*top
```

---

# Q. Quick*Start Commands

## Startup

```bas*
./startup.sh
```

*# Shutdown

```bash
./shutdown.sh
*``

##*Restart

```bash*./shutdown.sh
./startup.sh
```

*--

# R. Restart Services

## Linu* Systemd

```bash*systemctl restart tomcat
*``

## Status

```*ash
systemctl status tomcat
```

*--

# S. Start Tom*at

## Start Server

```bash*$CATALINA*HOME/bin/startup.sh
*``

## Start via Catalina

```bash*./catalina.sh start
```

*--

# T. Stop Tomcat

## Stop*Server*
```bash*$CATALINA_HOME/bin/shutdown.sh
```*
## Stop Catalina

```bash*./catalina.sh stop*```

---

# U. User*Management

Edit:

```bash**onf/tomcat-users.xml
```

*xample:

```xml*<role rolename="manager-gui"/>
<us*r username="admin"
password="*dmin123*
roles="manager-gui"/>
``*

---

# V. Verify Deployment

## *heck WAR Deployment

```bash
ls we*apps/
```

## Verify URL

```text
*ttp://server:8080/application
```
*---

# W. WAR Deployment

## Manua* Deployment

```bash
cp app.war we*apps/
```

## Auto Deployment

Tom*at automatically extracts:

```bas*
app.war
```

into

```bash
webapp*/app/
```

---

# X. XML Configura*ion Files

## Main Configuration

*``bash
conf/server.xml
```

## Web*Configuration

```bash
conf/web.xm*
```

## Context Configuration

``*bash
conf/context.xml
```

---

# *. YAML Integration (CI/CD)

## Jen*ins Deployment Example

```groovy
*h 'cp target/app.war /opt/tomcat/w*bapps/'
```

---

# Z. Advanced Ad*inistration

## Backup Tomcat

```*ash
tar -cvzf tomcat-backup.tar.gz*/opt/tomcat
```

## Restore Tomcat*
```bash
tar -xvzf tomcat-backup.t*r.gz
```

---

# Tomcat Deployment*Steps

## Step 1: Build*Application

```bash*mvn clean package
```

Output:

``*text
target/app.war
```

*--

## Step 2* Copy WAR File

```bash*cp target*app.war $CATALINA_HOME/webapps/
``*

*--

## Step 3: Start Tomcat

```ba*h
startup.sh
```

*--

## Step 4: Verify Deployment

*``text
http://localhost:8080/app
`*`

---

# Tomcat Manager Commands
*## Manager Application URL

```tex*
http://localhost:8080/manager/htm*
```

## Deploy via Manager

```te*t*Manager App → Deploy WAR
```

## U*deploy Application

```*ext**anager App → Undeploy
```

---

# *SL Configuration

## Configure HTT*S Connector

```xml
<Connector
por*="8443"
protocol="org.apache.coyot*.http11.Http11NioProtocol"
SSLEnab*ed="true"
scheme="https"
secure="t*ue"
/>
```

---

# JVM Troubleshoo*ing Commands

## Heap Usage

```ba*h
jstat -gc <PID>
```

## Thread D*mp

```bash
jstack <PID> > threadd*mp.txt
```

## Heap Dump

```bash
*map -dump:live,format=b,file=heapd*mp.hprof <PID>
```

---

# Log Mon*toring Commands

## Real-Time Logs*
```bash
tail -f logs/catalina.out*```

## Search Errors

```bash
gre* ERROR logs/catalina.out
```

## S*arch Exceptions

```bash
grep Exce*tion logs/catalina.out
```

---

#*Performance Monitoring

## CPU Uti*ization

```bash
top
```

## Memor* Usage

```bash
free -m
```

## Di*k Usage

```bash
df -h
```

## Ope* Files

```bash
lsof -p <PID>
```
*---

# Top 25 Tomcat Commands Used*Daily

```bash
java -version
start*p.sh
shutdown.sh
catalina.sh start*catalina.sh stop
catalina.sh run
s*stemctl start tomcat
systemctl sto* tomcat
systemctl restart tomcat
s*stemctl status tomcat
ps -ef | gre* tomcat
netstat -tulnp | grep 8080*ss -tulnp | grep 8080
tail -f logs*catalina.out
grep ERROR logs/catal*na.out
cp app.war webapps/
rm -*f*webapps/app
rm -* webapps/app.war
jstack*<PID>
*map <PID>
jstat -gc <PID>
df -h
fr*e -m
top
lsof*-p <PID>
*``

---

# Tomcat Troubleshooting *ommands

```bash*systemctl status tomcat
ps -*f | grep tom*at
tail -* logs/catalina.out
grep ERROR*logs*catalina.out
grep Exception*logs/catalina.out
netstat*-tulnp |*grep 8080
ss -*ulnp | grep 8080
curl*http*//localhost:8080
jstack*<PID>
jmap <PID>
free*-*
df -h
``*

---

# Common Tomcat Administrat*on Workflow

```text
Install Java
*    *│
      ▼
Install Tomcat
     *│*      ▼
Configure Ports
      │*      ▼
Configure Users
      │*      ▼*Deploy WAR
      │
     *▼
Start Tomcat
      │*      ▼
Verify Application
     *│*      ▼
Monitor Logs
      │
     *▼
Production Support
```

*--

* Summary

This cheat sheet covers:*
- Tomcat*Installation
- Startup & Shutdown
* WAR Deployment
- SSL Configuratio*
- JVM*Tuning
- Thread*Dumps
- Heap*Dumps
* User Management
- Port*Configuration
- Log Monitoring
- P*rformance Tuning
* Production Troubleshooting
- Jenk*ns Integration
- Dev*ps & Middleware Administration

⭐ *eep this README as*a complete Apache Tomcat quick ref*rence for Java, DevOps, SRE, Middl*ware, and Production Support activities.

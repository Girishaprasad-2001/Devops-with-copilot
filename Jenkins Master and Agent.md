# Jenkins Master and Agent (Slave) A to Z Guide
## Architecture, Configuration, Workflow & Administration

> **Note:** Modern Jenkins terminology uses **Controller** and **Agent** instead of **Master** and **Slave**. However, many organizations and interview questions still use the older terminology, so both terms are referenced in this guide.

---

# Table of Contents

1. Jenkins Architecture
2. Master vs Agent
3. Communication Workflow
4. Agent Types
5. Jenkins Installation
6. Configure Master
7. Configure Agent
8. SSH Agent Setup
9. JNLP Agent Setup
10. Pipeline Execution Flow
11. Labels and Node Management
12. Distributed Build Architecture
13. Production Best Practices
14. Troubleshooting Commands
15. Interview Questions
16. End-to-End Workflow

---

# Jenkins Architecture

```text
                 +------------------+
                 | Jenkins Master   |
                 | (Controller)     |
                 +--------+---------+
                          |
          --------------------------------
          |              |              |
          ▼              ▼              ▼

     +---------+    +---------+    +---------+
     | Agent-1 |    | Agent-2 |    | Agent-3 |
     | Linux   |    | Docker  |    | Windows |
     +---------+    +---------+    +---------+

          |              |              |
          ▼              ▼              ▼

      Build         Test Jobs      Deploy Jobs
```

---

# What is Jenkins Master?

The Jenkins Master (Controller) is responsible for:

- Managing Jenkins UI
- Scheduling Builds
- Maintaining Job Configurations
- Managing Users
- Managing Plugins
- Assigning Jobs to Agents
- Centralized Monitoring

---

# What is Jenkins Agent (Slave)?

An Agent is a machine that executes Jenkins jobs.

Examples:

```text
Linux Server
Windows Server
Docker Container
Kubernetes Pod
Cloud VM
```

Agent Responsibilities:

- Build Applications
- Run Tests
- Execute Scripts
- Deploy Applications

---

# Why Use Agents?

Without Agents:

```text
All jobs run on Jenkins Server
```

Problems:

```text
High CPU Usage
Memory Issues
Scalability Problems
```

With Agents:

```text
Distribute Workloads
Parallel Execution
Faster Builds
Resource Isolation
```

---

# Jenkins Distributed Build Workflow

```text
Developer Commit
        |
        ▼

GitHub/GitLab
        |
        ▼

Jenkins Master
        |
        ▼

Assign Job
        |
        ▼

Available Agent
        |
        ▼

Execute Build
        |
        ▼

Return Results
        |
        ▼

Jenkins UI
```

---

# Agent Types

## SSH Agent

Most Common

```text
Jenkins Master ---> SSH ---> Linux Agent
```

---

## JNLP Agent

Java Based Agent

```text
Agent Connects To Jenkins
```

---

## Windows Agent

```text
Windows Service
```

---

## Docker Agent

```text
Temporary Container
```

---

## Kubernetes Agent

```text
Dynamic Pod
```

---

# Step 1: Install Jenkins Master

## Install Java

```bash
sudo yum install java-17-openjdk -y
```

Verify:

```bash
java -version
```

---

## Install Jenkins

```bash
wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

```bash
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

```bash
yum install jenkins -y
```

---

## Start Jenkins

```bash
systemctl enable jenkins
```

```bash
systemctl start jenkins
```

```bash
systemctl status jenkins
```

---

## Verify Port

```bash
ss -tulnp | grep 8080
```

Access:

```text
http://SERVER-IP:8080
```

---

# Step 2: Setup Linux Agent

## Create Jenkins User

```bash
sudo useradd jenkins
```

Set Password:

```bash
sudo passwd jenkins
```

---

## Install Java

```bash
sudo yum install java-17-openjdk -y
```

Verify:

```bash
java -version
```

---

# Step 3: SSH Key Configuration

On Master:

```bash
su - jenkins
```

Generate Key:

```bash
ssh-keygen
```

---

## Copy Key to Agent

```bash
ssh-copy-id jenkins@agent-server
```

Verify:

```bash
ssh jenkins@agent-server
```

Should Login Without Password.

---

# Step 4: Add Agent in Jenkins

Navigate:

```text
Manage Jenkins
    |
    ▼
Nodes
    |
    ▼
New Node
```

---

## Agent Configuration

```text
Node Name : linux-agent

Type : Permanent Agent
```

---

## Remote Root Directory

```text
/home/jenkins
```

---

## Labels

```text
linux
maven
docker
```

---

## Launch Method

```text
Launch agents via SSH
```

---

## Host

```text
192.168.1.100
```

---

## Credentials

```text
SSH Username + Private Key
```

Save.

---

# Jenkins Agent Connection Flow

```text
Jenkins Master
      |
      ▼
SSH Connection
      |
      ▼
Java Check
      |
      ▼
Agent.jar Download
      |
      ▼
Agent Online
```

---

# Verify Agent

Navigate:

```text
Manage Jenkins
     |
     Nodes
```

Expected:

```text
Agent Online
```

Green Status.

---

# JNLP Agent Configuration

Navigate:

```text
Manage Jenkins
   |
   Nodes
   |
   New Node
```

Select:

```text
Launch Agent by Java Web Start
```

---

## Download Agent

```bash
wget http://jenkins-server:8080/jnlpJars/agent.jar
```

---

## Connect Agent

```bash
java -jar agent.jar \
-jnlpUrl http://jenkins:8080/computer/linux-agent/slave-agent.jnlp \
-secret SECRET_KEY
```

---

# Labels Usage

Configure:

```text
linux
docker
java
maven
kubernetes
```

Pipeline:

```groovy
pipeline {

 agent {
   label 'docker'
 }

 stages {

   stage('Build') {
     steps {
       sh 'docker version'
     }
   }

 }
}
```

---

# Multiple Agent Architecture

```text
                    Jenkins Master
                           |
      -------------------------------------------
      |                  |                      |
      ▼                  ▼                      ▼

 Linux Agent      Windows Agent         Docker Agent

 Maven Jobs       .NET Jobs             Container Builds
```

---

# Build Execution Workflow

```text
Job Trigger
      |
      ▼

Master Scheduler

      |
      ▼

Find Matching Label

      |
      ▼

Agent Assignment

      |
      ▼

Execute Job

      |
      ▼

Logs Sent Back

      |
      ▼

Build Success/Failure
```

---

# Pipeline Example

```groovy
pipeline {

 agent {
   label 'linux'
 }

 stages {

   stage('Checkout') {

     steps {
       git 'https://github.com/company/project.git'
     }
   }

   stage('Build') {

     steps {
       sh 'mvn clean package'
     }
   }

   stage('Test') {

     steps {
       sh 'mvn test'
     }
   }
 }
}
```

---

# Master-Agent Communication Ports

```text
8080 Jenkins Web UI
50000 Agent Communication
22 SSH
```

Verify:

```bash
ss -tulnp | grep 50000
```

---

# Docker Agent Example

```groovy
pipeline {

 agent {

   docker {
      image 'maven:3.9.8-eclipse-temurin-17'
   }

 }

 stages {

   stage('Build') {

     steps {

       sh 'mvn clean package'

     }

   }

 }
}
```

---

# Kubernetes Agent Example

```groovy
pipeline {

 agent {

   kubernetes {

      label 'k8s-agent'

   }

 }

 stages {

   stage('Build') {

     steps {

       sh 'mvn clean install'

     }

   }

 }
}
```

---

# Jenkins Security Best Practices

## Disable Root Usage

```text
Use Dedicated Jenkins User
```

---

## Enable RBAC

```text
Role-Based Access Control
```

---

## Use Credentials Store

Never Store:

```text
Passwords
API Keys
Tokens
```

Inside Pipelines.

---

## Integrate Vault

```text
HashiCorp Vault
```

For Secrets Management.

---

# Backup Jenkins

## Jenkins Home

```bash
tar -czvf jenkins-backup.tar.gz \
/var/lib/jenkins
```

Restore:

```bash
tar -xzvf jenkins-backup.tar.gz
```

---

# Monitoring Jenkins

## Service Status

```bash
systemctl status jenkins
```

## Logs

```bash
journalctl -u jenkins -f
```

## Process

```bash
ps -ef | grep jenkins
```

---

# Troubleshooting Agents

## Check SSH

```bash
ssh jenkins@agent-server
```

## Check Java

```bash
java -version
```

## Check Agent Process

```bash
ps -ef | grep agent
```

## Jenkins Logs

```bash
journalctl -u jenkins -f
```

## Verify Port

```bash
ss -tulnp | grep 50000
```

---

# Top Daily Jenkins Administration Commands

```bash
systemctl start jenkins
systemctl stop jenkins
systemctl restart jenkins
systemctl status jenkins

journalctl -u jenkins -f

java -version

ssh agent-server

ps -ef | grep java

df -h

free -m

top

ss -tulnp | grep 8080

ss -tulnp | grep 50000

tar -czvf jenkins-backup.tar.gz /var/lib/jenkins
```

---

# Common Interview Workflow

```text
Developer Commit
       |
       ▼

GitHub

       |
       ▼

Webhook Trigger

       |
       ▼

Jenkins Master

       |
       ▼

Select Agent

       |
       ▼

Execute Build

       |
       ▼

Run Tests

       |
       ▼

Build Artifact

       |
       ▼

Deploy

       |
       ▼

Success/Failure Notification
```

---

# Jenkins Master vs Agent

## Master (Controller)

```text
Job Scheduling
User Management
Plugin Management
Credential Storage
Build Coordination
```

## Agent (Slave)

```text
Build Execution
Testing
Packaging
Deployment
Automation Scripts
```

---

# Production Architecture

```text
                 Load Balancer
                       |
                       ▼

              Jenkins Controller

                       |
        --------------------------------

        ▼              ▼             ▼

   Linux Agent     Docker Agent   K8s Agent

        ▼              ▼             ▼

      Build          Images       Deployments

                       |
                       ▼

                Production
```

---

# Summary

This guide covers:

- Jenkins Controller (Master)
- Jenkins Agent (Slave)
- SSH Agents
- JNLP Agents
- Docker Agents
- Kubernetes Agents
- Agent Labels
- Distributed Builds
- Pipeline Execution
- Security Best Practices
- Backup & Restore
- Monitoring
- Troubleshooting
- Production Architecture

⭐ Keep this README as a complete Jenkins Controller-Agent reference for DevOps Engineers, CI/CD Engineers, SREs, Platform Engineers, and Production Support teams.

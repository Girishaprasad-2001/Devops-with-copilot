# Jenkins A to Z Commands & Pipeline Steps Cheat Sheet

A complete Jenkins reference covering Jenkins CLI commands, administration tasks, job management, agent operations, pipeline syntax, Declarative and Scripted Pipelines, and CI/CD best practices.

---

# A. Access Jenkins CLI

## Download Jenkins CLI

```bash
wget http://<jenkins-url>/jnlpJars/jenkins-cli.jar
```

## Test CLI Connection

```bash
java -jar jenkins-cli.jar -s http://<jenkins-url> version
```

---

# B. Build Jobs

## Trigger Build

```bash
java -jar jenkins-cli.jar -s http://localhost:8080 build my-job
```

## Build and Wait

```bash
java -jar jenkins-cli.jar -s http://localhost:8080 build my-job -s
```

## Build with Parameters

```bash
java -jar jenkins-cli.jar \
-s http://localhost:8080 \
build my-job \
-p ENV=dev
```

---

# C. Create Jobs

## Create Job

```bash
java -jar jenkins-cli.jar create-job my-job < config.xml
```

## Copy Job

```bash
java -jar jenkins-cli.jar copy-job old-job new-job
```

---

# D. Delete Jobs

## Delete Job

```bash
java -jar jenkins-cli.jar delete-job my-job
```

## Disable Job

```bash
java -jar jenkins-cli.jar disable-job my-job
```

## Enable Job

```bash
java -jar jenkins-cli.jar enable-job my-job
```

---

# E. Execute Groovy Scripts

## Run Groovy Script

```bash
java -jar jenkins-cli.jar groovy script.groovy
```

## Run Groovy Command

```groovy
println Jenkins.instance.version
```

---

# F. Fetch Information

## Jenkins Version

```bash
java -jar jenkins-cli.jar version
```

## List Jobs

```bash
java -jar jenkins-cli.jar list-jobs
```

## View Job Info

```bash
java -jar jenkins-cli.jar get-job my-job
```

---

# G. Get Build Logs

## Console Output

```bash
java -jar jenkins-cli.jar console my-job
```

## Last Build Logs

```bash
java -jar jenkins-cli.jar console my-job -n 100
```

---

# H. History Commands

## List Builds

```bash
curl http://localhost:8080/job/my-job/api/json
```

## Last Successful Build

```bash
curl http://localhost:8080/job/my-job/lastSuccessfulBuild/api/json
```

---

# I. Install Plugins

## Install Plugin

```bash
java -jar jenkins-cli.jar install-plugin git
```

## Install Multiple Plugins

```bash
java -jar jenkins-cli.jar install-plugin git workflow-aggregator
```

## Restart After Install

```bash
java -jar jenkins-cli.jar safe-restart
```

---

# J. Jenkins Restart Commands

## Safe Restart

```bash
java -jar jenkins-cli.jar safe-restart
```

## Restart Jenkins Service

```bash
systemctl restart jenkins
```

## Stop Jenkins

```bash
systemctl stop jenkins
```

## Start Jenkins

```bash
systemctl start jenkins
```

---

# K. Kill Running Builds

## Stop Build

```bash
java -jar jenkins-cli.jar stop-build my-job 15
```

---

# L. List Agents

## List Nodes

```bash
java -jar jenkins-cli.jar list-nodes
```

## Get Node Details

```bash
java -jar jenkins-cli.jar get-node agent1
```

---

# M. Manage Credentials

## List Credentials (Groovy)

```groovy
com.cloudbees.plugins.credentials.CredentialsProvider.lookupCredentials(
com.cloudbees.plugins.credentials.common.StandardCredentials.class,
Jenkins.instance,
null,
null
)
```

---

# N. Node Commands

## Online Node

```bash
java -jar jenkins-cli.jar online-node agent1
```

## Offline Node

```bash
java -jar jenkins-cli.jar offline-node agent1
```

---

# O. Obtain System Information

## System Info

```bash
http://localhost:8080/systemInfo
```

## Environment Variables

```bash
http://localhost:8080/env-vars.html
```

---

# P. Plugin Management

## List Plugins

```bash
java -jar jenkins-cli.jar list-plugins
```

## Uninstall Plugin

```bash
java -jar jenkins-cli.jar uninstall-plugin plugin-name
```

---

# Q. Queue Management

## View Queue

```bash
http://localhost:8080/queue
```

## Cancel Queue Item

```groovy
Jenkins.instance.queue.clear()
```

---

# R. Reload Configuration

## Reload Jenkins

```bash
java -jar jenkins-cli.jar reload-configuration
```

---

# S. Security Commands

## Generate API Token

```text
Manage Jenkins → Users → Configure → API Token
```

## Check Authorization Settings

```text
Manage Jenkins → Security
```

---

# T. Trigger Jobs

## Build Job Remotely

```bash
curl -X POST \
"http://localhost:8080/job/my-job/build?token=mytoken"
```

---

# U. Update Center

## Check Updates

```bash
http://localhost:8080/pluginManager
```

---

# V. View Jenkins Logs

## Jenkins Service Logs

```bash
journalctl -u jenkins -f
```

## Docker Logs

```bash
docker logs -f jenkins
```

---

# W. Workspace Management

## Delete Workspace

```groovy
Jenkins.instance.getItem("job-name").workspace.deleteRecursive()
```

---

# X. Export Job Configuration

## Get Config.xml

```bash
java -jar jenkins-cli.jar get-job my-job > config.xml
```

---

# Y. YAML / JCasC

## Jenkins Configuration as Code

```yaml
jenkins:
  systemMessage: "Managed by JCasC"
```

---

# Z. Advanced Administration

## Backup Jenkins Home

```bash
tar -cvzf jenkins_backup.tar.gz /var/lib/jenkins
```

## Restore Jenkins Home

```bash
tar -xvzf jenkins_backup.tar.gz
```

---

# Jenkins Declarative Pipeline

## Basic Pipeline

```groovy
pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building Application'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Tests'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application'
            }
        }
    }
}
```

---

# Jenkins Scripted Pipeline

```groovy
node {

    stage('Build') {
        echo 'Build Stage'
    }

    stage('Test') {
        echo 'Test Stage'
    }

    stage('Deploy') {
        echo 'Deploy Stage'
    }
}
```

---

# Important Pipeline Steps

## Checkout Source Code

```groovy
checkout scm
```

```groovy
git branch: 'main',
url: 'https://github.com/org/repo.git'
```

---

## Execute Shell Commands

```groovy
sh 'mvn clean package'
```

---

## Execute Batch Commands

```groovy
bat 'dir'
```

---

## Echo Messages

```groovy
echo 'Pipeline Started'
```

---

## Environment Variables

```groovy
environment {
    APP_ENV = "dev"
}
```

Usage:

```groovy
echo "${APP_ENV}"
```

---

## Credentials

```groovy
environment {
    AWS_CREDS = credentials('aws-secret')
}
```

---

## Archive Artifacts

```groovy
archiveArtifacts artifacts: '**/*.jar'
```

---

## Publish Test***sults

```groovy
junit '**/targe***urefire-reports/*.xml'
```

---
*** Input Approval

```groovy
input***pprove Deployment?'
```

---

##***meout

```groovy
timeout(time: 1***unit: 'MINUTES') {
    sh 'sleep***'
}
```

---

## Retry

```groov***etry(3) {
    sh './deploy.sh'
}***`

---

## Parallel Execution

`***roovy
parallel(
    Build: {
   ***  sh 'mvn package'
    },
    Te*** {
        sh 'mvn test'
    }
)***`

---

## Docker Build

```groo***sh 'docker build -t myapp .'
```***--

## Docker Push

```groovy
sh***ocker push myrepo/myapp:latest'
***

---

## Kubernetes Deployment
***`groovy
sh 'kubectl apply -f dep***ment.yaml'
```

---

# Complete ***CD Pipeline Example

```groovy
p***line {

    agent any

    tools***        maven 'Maven3'
    }

  ***tages {

        stage('Checkout***{
            steps {
          ***   git branch: 'main',
         ***    url: 'https://github.com/org***po.git'
            }
        }
***      stage('Build') {
         ***steps {
                sh 'mvn ***an package'
            }
      ***

        stage('Test') {
      ***   steps {
                sh 'm***test'
            }
        }

 ***    stage('Docker Build') {
    ***     steps {
                sh ***cker build -t myapp .'
         ***}
        }

        stage('Depl***) {
            steps {
        ***     sh 'kubectl apply -f deploy***t.yaml'
            }
        }
*** }
}
```

---

# Top 25 Jenkins ***mands Used Daily

```bash
system*** status jenkins
systemctl start ***kins
systemctl stop jenkins
syst***tl restart jenkins
journalctl -u***nkins -f

java -jar jenkins-cli.*** list-jobs
java -jar jenkins-cli***r build my-job
java -jar jenkins***i.jar list-plugins
java -jar jen***s-cli.jar safe-restart

curl htt***/localhost:8080/job/my-job/api/j***

docker logs -f jenkins
docker ***tart jenkins

kubectl logs -f je***ns-pod
kubectl describe pod jenk***-pod

git pull
mvn clean install***ocker build -t app .
docker push***p

kubectl apply -f deployment.y***

archiveArtifacts
junit

checko***scm
sh
retry
timeout
```

---

#***nkins Troubleshooting Commands

***bash
systemctl status jenkins
jo***alctl -u jenkins -f

docker ps
d***er logs jenkins

java -jar jenki***cli.jar list-jobs
java -jar jenk***-cli.jar list-plugins

curl http***localhost:8080/login

df -h
free***
top

kubectl get pods
kubectl d***ribe pod jenkins
kubectl logs jenkins
```

---

# Common Jenkins CI/CD Workflow

```text
Developer Commit
        │
        ▼
GitHub/GitLab
        │
        ▼
Webhook Trigger
        │
        ▼
Jenkins Pipeline
        │
 ┌──────┼──────┐
 ▼      ▼      ▼
Build  Test  Scan
        │
        ▼
Docker Build
        │
        ▼
Docker Push
        │
        ▼
Kubernetes Deploy
        │
        ▼
Production
```

---

# Summary

This cheat sheet covers:

- Jenkins Administration
- Jenkins CLI Commands
- Plugin Management
- Agent Management
- Backup & Restore
- Declarative Pipelines
- Scripted Pipelines
- CI/CD Automation
- Git Integration
- Maven Builds
- Docker Builds
- Kubernetes Deployment
- Production Troubleshooting

⭐ Keep this README as a complete Jenkins quick reference for DevOps, SRE, Platform Engineering, and CI/CD activities.

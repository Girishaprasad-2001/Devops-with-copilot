# GitHub Actions A to Z Guide
## Commands, Workflow, Configuration & Step-by-Step Setup

A complete GitHub Actions reference covering:

- GitHub Actions Fundamentals
- Workflow Setup
- CI/CD Pipelines
- Build Automation
- Docker Integration
- Kubernetes Deployment
- Secrets Management
- Self-Hosted Runners
- Reusable Workflows
- Matrix Builds
- DevSecOps Integration
- Troubleshooting
- Best Practices

---

# What is GitHub Actions?

GitHub Actions is GitHub's native CI/CD platform that allows you to:

- Automate Build Processes
- Run Tests
- Deploy Applications
- Perform Security Scans
- Automate Infrastructure
- Implement GitOps Workflows

---

# GitHub Actions Architecture

```text
Developer
    |
    ▼

Git Commit
    |
    ▼

GitHub Repository
    |
    ▼

GitHub Actions Workflow
    |
    ├── Build
    ├── Test
    ├── Scan
    ├── Package
    └── Deploy
    |
    ▼

Target Environment
```

---

# Workflow Lifecycle

```text
Push Event
     |
     ▼

Workflow Trigger
     |
     ▼

Runner
     |
     ▼

Job
     |
     ▼

Steps
     |
     ▼

Deployment
```

---

# GitHub Actions Directory Structure

```text
project/

├── src/
├── pom.xml
└── .github
    └── workflows
         ├── build.yml
         ├── deploy.yml
         └── security.yml
```

---

# A. Actions Basics

## Workflow File Location

```text
.github/workflows/
```

Example:

```text
.github/workflows/build.yml
```

---

# B. Basic Workflow

```yaml
name: First Workflow

on:
  push:

jobs:

  build:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Hello World
        run: echo "Hello GitHub Actions"
```

---

# C. Common Triggers

## Push

```yaml
on:
  push:
```

---

## Pull Request

```yaml
on:
  pull_request:
```

---

## Multiple Events

```yaml
on:
  push:
  pull_request:
```

---

## Manual Trigger

```yaml
on:
  workflow_dispatch:
```

---

## Scheduled Job

```yaml
on:
  schedule:
    - cron: "0 2 * * *"
```

*uns daily at 2 AM*

---

# D. Environment Variables
*```yaml
env:
  APP*NAME: myapp
*``

Usage:

```yaml*run: echo $APP_NAME
```

*--

# E. Expressions

```yaml*if: github.ref == 'refs/heads/main*
```

---

# F. Filters

Trigger o*ly for main branch:

```yaml*on:
  push:
    branches:
     *- main
```

---

# G. Git*Checkout*
```yaml*- uses: actions/checkout@*4
```

---

# H. Hosted*Runners

```*aml
runs-on: ubuntu-latest
``*

*vailable:

```text*ubuntu-latest
windows-latest
*acos-latest
```

*--

# I. Inputs

Manual*Workflow Input

```yaml
on:
  work*low_dispatch:
    inputs:
      en*:
        required: true
```

Usag*:

```yaml*${{ github.event.inputs.env }}
```*
---

# J. Jobs

```yaml
jobs:

  *uild:

   *runs-on: ubuntu-latest
```

*--

# K. Kubernetes Deployment*
```yaml*- name: Deploy
  run: |
    kub*ctl apply -f deployment.yaml
```

*--

# L. Logs

Workflow logs*available under:

```text
Git*ub Repository
 *|
  Actions*  |
  Workflow Run
```

*--

# M. Matrix Builds

Run*multiple*versions:

```yaml*strategy:

  matrix:

    java-ver*ion:
      - 17
      - 21
``*

---

# N. Notifications*
*lack Example*

```yaml*- name: Slack Notification
  run* echo "Notify Slack"
```

*--

# O. Outputs

```yaml*outputs:
  image*tag: $*{ steps.build.outputs.tag*}}
```

---

# P. Permissions*
```*aml
permissions:
  contents* read
```

---

# Q. Quality*Checks

Son*rQube

```yaml*- name: SonarQube Scan
  run* mvn sonar:sonar
```

*--

* R. Reusable Workflow

```yaml*uses: company*repo/.*ithub/workflows/build.yml@main
```*
---

# S. Secrets

Store Secret:
*```text
Settings
   |
   Secrets*and*Variables
   |
   Actions
```

Usa*e:

```yaml
${{ secrets.DOCKER_PAS*WORD }}
```

---

# T. Testing

``*yaml
- name: Run Tests
  run: mvn *est
```

---

# U. Upload Artifact*

```yaml
- uses: actions/upload-a*tifact@v4

  with:
    name: build*output
    path: target/
```

*--

# Download Artifacts

```yaml*- uses: actions/download-artifact@*4
```

---

# V. Variables

Reposi*ory Variables:

```yaml
${{ vars.A*P_NAME }}
```

---

# W. Workflow *ispatch

Manual Execution*
```yaml
on:
  workflow_dispatch:
*``

---

# X.*External Actions

```yaml
uses* docker*login-action@*3
```

```yaml
uses:*actions/setup-java*v4
```

---

# Y.*YAML Best Practices

Use descripti*e names.

```yaml
name: Build Java*Application
```

---

# Z. Advance**Workflows

Sequential*Jobs

```yaml
jobs:

  build:

  t*st:
    needs* build

  deploy:
    needs: test
*``

---

# Java Maven Build*Workflow

```yaml
name: Java CI

o*:
  push:

jobs:

* build:

    runs-on: ubuntu-lates*

    steps:

    - uses: actions/*heckout@v4

    - uses: actions/se*up-java@v4

      with:
        di*tribution* temurin
        java*version: 17

    -*run: mvn clean package
```

*--

#*Docker Build Workflow

```yaml*name: Docker Build

on:
  push:

j*bs:

 *docker:

    runs-on: ubuntu-lates*

    steps:

    - uses: actions/*heckout@v4

    - run: docker buil* -t app:v1 .
```

---

# Docker Pu*h Workflow

```yaml
- name: Login *ockerHub

  uses: docker/login-act*on@v3

  with:

    username: ${{ *ecrets.DOCKER_USERNAME }}

    pas*word: ${{ secrets.DOCKER_PASSWORD *}

- name: Push

  run: docker pus* myrepo/app:v1
```

---

# Kuberne*es Deployment Workflow

```yaml
na*e: Deploy

on:
  push:

jobs:

  d*ploy:

    runs-on: ubuntu-latest
*    steps:

    - uses: actions/ch*ckout@v4

    - name: Deploy

    * run: |
        kubectl apply -f d*ployment.yaml
```

---

# CI/CD En*-to-End Workflow

```text
Develope* Commit
      |
      ▼

GitHub Re*ository
      |
      ▼

GitHub Ac*ions Trigger
      |
      ▼

Buil*
      |
      ▼

Test
      |
   *  ▼

Code Quality Check
      |
  *   ▼

Security Scan
      |
      *

Docker Build
      |
      ▼

Do*ker Push
      |
      ▼

Kubernet*s Deploy
      |
      ▼

Producti*n
```

---

# DevSecOps Workflow

*``yaml
name: Secure Pipeline

on:
* push:

jobs:

  security:

    ru*s-on: ubuntu-latest

    steps:

 *  - uses: actions/checkout@v4

   *- name: Secret Scan

      run: gi*leaks detect

    - name: Dependen*y Scan

      run: mvn dependency-*heck:check

    - name: Trivy Scan*
      run: trivy fs .
```

---

#*Self-Hosted Runner Setup

## Downl*ad Runner

```bash
mkdir actions-r*nner

cd actions-runner
```

```ba*h
curl -o runner.tar.gz <runner-ur*>
```

---

## Configure

```bash
*/config.sh
```

*--

## Start Runner

```bash*./run.sh
```

---

# GitHub Action* Commands

## GitHub CLI Login

``*bash
gh auth login
```

---

## Wo*kflow List

```bash
gh workflow li*t
```

*--

## View Workflow

```bash*gh workflow view
```

---

## Run *orkflow

```bash
gh workflow run b*ild.yml
```

---

## View Runs

*``bash
gh run list
```

---

## Vi*w Run Details

```bash
*h run view
```

*--

## Watch Workflow

*``bash
gh run watch
```

*--

## Download Logs

```bash*gh run download
*``

*--

# Top Daily GitHub Actions Com*ands

```bash
gh auth login

gh wo*kflow list

gh workflow view

gh w*rkflow run

gh run list

gh run vi*w

gh run watch

gh run*download

git add .

git commit*-m "update"

git push*origin main
*``

*--

# GitHub Actions Best Practice*

```text
*se Secrets Instead of Plain Text

*se Branch*Protection

Enable Code*Scanning

Use Least*Privilege Permissions

Implement*En*ironments

Use Re*sable Workflows

Scan Containers

*can Dependencies

Implement Approv*l*Gates
```

*--

# Troubleshooting

## Workflow*Failure

Check*

```text
GitHub
 |
*Actions
 |
 Workflow Run* |
 Logs
```

*--

## Validate*YAML

```yaml
yam*lint .github/workflows/build*yml
```

*--

## Debug Variables

```yaml*- run: env
```

*--

#*Enterprise GitHub Actions Flow

*``text
Developer
    *|
     ▼

GitHub*
     |
     ▼

GitHub Actions

  *  |
     ├──*Build
*    ├── Test
     ├── SonarQube
  * *├── Trivy
     ├*─ Gitleaks
    *├── Docker Build
*    └*─ Docker Push

     |
     ▼*
Artifact Registry

     |
    *▼

Ar*o*D

     |
     ▼

Kubernetes*
     |
     ▼

Production**``

---

# Summary

This guide*covers:

- GitHub Actions Fundamen*als
- Workflow Creation
- Events &*Triggers
- Jobs & Steps
- Variable* & Secrets
* Artifacts
- Matrix Builds
- Docke* Integration
- Kubernetes Deployme*t
- Self-Hosted*Runners
- GitHub CLI
- Dev*ecOps Integration
- CI*CD Best Practices
* Enterprise*Deployment Workflow

⭐ Keep*this README as a complete Git*ub Actions reference for CI*CD, DevOps, DevSecOps, GitOps, Clo*d Engineering, and Platform Engine*ring.

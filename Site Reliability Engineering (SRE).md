# Site Reliability Engineering (SRE)
# A to Z Guide: Architecture, Configuration, Workflow & Administration

A complete SRE reference covering:

- SRE Fundamentals
- Reliability Engineering
- SLI, SLO, SLA
- Incident Management
- Monitoring & Observability
- Capacity Planning
- Automation
- DevOps & SRE Integration
- Kubernetes Reliability
- Cloud Reliability
- Error Budgets
- On-Call Operations
- Production Support
- Disaster Recovery
- High Availability
- Root Cause Analysis (RCA)

---

# What is SRE?

Site Reliability Engineering (SRE) is a discipline that applies software engineering principles to IT operations.

Goal:

```text
Improve Reliability
Reduce Downtime
Automate Operations
Increase Scalability
Improve Performance
```

---

# SRE Core Principles

```text
Automation First
Observability
Reliability
Scalability
Error Budgets
Service Ownership
Continuous Improvement
```

---

# SRE Architecture

```text
Users
  |
  ▼

Applications

  |
  ▼

Load Balancer

  |
  ▼

Kubernetes

  |
  ▼

Application Pods

  |
  ▼

Database

  |
  ▼

Monitoring Stack
```

---

# SRE Responsibilities

## Reliability

```text
Ensure Service Availability
Reduce Failures
Maintain SLAs
```

---

## Monitoring

```text
Monitor Infrastructure
Monitor Applications
Monitor Databases
```

---

## Incident Management

```text
Detect
Respond
Resolve
Review
```

---

## Automation

```text
Provision Infrastructure
Automate Deployments
Automate Recovery
```

---

# SRE Workflow

```text
Development

     |
     ▼

CI/CD

     |
     ▼

Production

     |
     ▼

Monitoring

     |
     ▼

Alerts

     |
     ▼

Incident Response

     |
     ▼

Root Cause Analysis
```

---

# SLI, SLO and SLA

---

## SLI

Service Level Indicator

Measures:

```text
Availability
Latency
Error Rate
Throughput
```

Example:

```text
99.95% Service Availability
```

---

## SLO

Service Level Objective

Target Reliability.

Example:

```text
99.9% Uptime
```

---

## SLA

Service Level Agreement

Business commitment.

Example:

```text
99.9% Uptime

Below This = Financial Penalty
```

---

# Relationship

```text
SLI → Measurement

SLO → Target

SLA → Business Contract
```

---

# Error Budget

Error Budget allows acceptable failures.

Example:

```text
SLO = 99.9%

Allowed Failure = 0.1%
```

---

# Error Budget Formula

```text
100 - SLO
```

Example:

```text
100 - 99.9

= 0.1%
```

---

# Monitoring Architecture

```text
Application
      |
      ▼

Node Exporter

      |
      ▼

Prometheus

      |
      ▼

Grafana

      |
      ▼

Alerts
```

---

# Golden Signals

Google SRE Golden Signals:

---

## Latency

```text
Response Time
```

---

## Traffic

```text
Requests Per Second
```

---

## Errors

```text
Error Rate
```

---

## Saturation

```text
CPU
Memory
Disk Usage
```

---

# Observability

Three Pillars:

```text
Metrics
Logs
Traces
```

---

# Metrics Stack

```text
Node Exporter

      |
      ▼

Prometheus

      |
      ▼

Grafana
```

---

# Logging Stack

```text
Application Logs

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

# Distributed Tracing

Tools:

```text
Jaeger
OpenTelemetry
Zipkin
Tempo
```

Architecture:

```text
Service A

    |
    ▼

Service B

    |
    ▼

Database

Trace Entire Request Journey
```

---

# Incident Management Lifecycle

```text
Incident Detected

        |
        ▼

Alert Generated

        |
        ▼

Engineer Assigned

        |
        ▼

Investigation

        |
        ▼

Fix

        |
        ▼

Validation

        |
        ▼

RCA
```

---

# Incident Severity Levels

## SEV-1

```text
Production Outage
Business Critical
```

---

## SEV-2

```text
Major Function Impact
```

---

## SEV-3

```text
Minor Impact
```

---

## SEV-4

```text
Informational
```

---

# Root Cause Analysis (RCA)

Template:

```text
Issue Summary

Timeline

Root Cause

Impact

Resolution

Preventive Actions
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

App-1       App-2

 |
 ▼

Database Primary

 |
 ▼

Database Replica
```

---

# Disaster Recovery

Objectives:

---

## RPO

Recovery Point Objective

```text
Maximum Data Loss Allowed
```

---

## RTO

Recovery Time Objective

```text
Maximum Recovery Time
```

---

# Example

```text
RPO = 15 Minutes

RTO = 30 Minutes
```

---

# Capacity Planning

Monitor:

```text
CPU
Memory
Storage
Network
Traffic Growth
```

---

# Capacity Workflow

```text
Current Utilization

      |
      ▼

Forecast

      |
      ▼

Scale Infrastructure

      |
      ▼

Prevent Outages
```

---

# Kubernetes SRE Architecture

```text
Users

  |
  ▼

Ingress

  |
  ▼

Services

  |
  ▼

Pods

  |
  ▼

Databases

  |
  ▼

Monitoring
```

---

# Kubernetes Reliability Checks

```bash
kubectl get nodes

kubectl get pods -A

kubectl top nodes

kubectl top pods
```

---

# Production Readiness Checklist

```text
Monitoring Enabled

Alerting Enabled

Backups Configured

HA Configured

Documentation Ready

Runbooks Available

Disaster Recovery Tested
```

---

# Runbooks

Runbooks provide standardized procedures.

Example:

```text
CPU High

1. Check Top Process
2. Verify Application Logs
3. Restart Service
4. Monitor Recovery
```

---

# Automation in SRE

Tools:

```text
Ansible

Terraform

Jenkins

GitHub Actions

ArgoCD
```

---

# Infrastructure Automation Workflow

```text
Git Commit

     |
     ▼

Terraform

     |
     ▼

Infrastructure

     |
     ▼

Monitoring
```

---

# DevOps vs SRE

## DevOps

Focus:

```text
Speed
Delivery
Automation
```

---

## SRE

Focus:

```text
Reliability
Availability
Performance
```

---

# DevOps + SRE Workflow

```text
Developer

    |
    ▼

CI/CD

    |
    ▼

Deployment

    |
    ▼

Monitoring

    |
    ▼

Reliability Validation
```

---

# SRE Tools

## Monitoring

```text
Prometheus

Grafana

Datadog

New Relic

CloudWatch
```

---

## Logging

```text
ELK

EFK

Splunk
```

---

## Tracing

```text
Jaeger

Zipkin

Tempo
```

---

## Incident Management

```text
PagerDuty

OpsGenie

ServiceNow
```

---

# Linux Commands for SRE

## CPU

```bash
top

htop

mpstat
```

---

## Memory

```bash
free -m

vmstat
```

---

## Disk

```bash
df -h

du -sh
```

---

## Network

```bash
ss -tulnp

netstat -tulnp

ip addr
```

---

## Logs

```bash
journalctl -xe

tail -f
```

---

# Kubernetes Commands

```bash
kubectl get pods

kubectl get deployments

kubectl describe pod

kubectl logs

kubectl top nodes

kubectl top pods
```

---

# Docker Commands

```bash
docker ps

docker logs

docker stats

docker images
```

---

# Database Checks

## MySQL

```sql
SHOW PROCESSLIST;
```

---

## PostgreSQL

```sql
SELECT * FROM pg_stat_activity;
```

---

# Troubleshooting Workflow

```text
Alert Triggered

      |
      ▼

Identify Service

      |
      ▼

Check Dashboard

      |
      ▼

Check Logs

      |
      ▼

Check Infrastructure

      |
      ▼

Implement Fix

      |
      ▼

Validate Recovery
```

---

# Daily SRE Activities

```text
Review Alerts

Review System Health

Verify Backups

Capacity Monitoring

Incident Response

RCA Reviews

Automation Improvements

Performance Tuning
```

---

# Enterprise SRE Architecture

```text
Users

   |
   ▼

Load Balancer

   |
   ▼

Kubernetes Cluster

   |
   ▼

Microservices

   |
   ▼

Database Cluster

   |
   ▼

Monitoring

   ├── Prometheus
   ├── Grafana
   ├── ELK
   └── Jaeger
```

---

# On-Call Workflow

```text
Alert

   |
   ▼

PagerDuty

   |
   ▼

SRE Engineer

   |
   ▼

Investigation

   |
   ▼

Resolution

   |
   ▼

Postmortem
```

---

# Production Support Model

```text
L1 Support
   |
   ▼
L2 Support
   |
   ▼
SRE Team
   |
   ▼
Engineering Team
```

---

# SRE Maturity Model

```text
Level 1
Reactive Operations

Level 2
Basic Monitoring

Level 3
Automation

Level 4
Reliability Engineering

Level 5
Full Observability

Level 6
Self-Healing Systems
```

---

# Key SRE Metrics

```text
Availability %

Error Rate

Response Time

P95 Latency

P99 Latency

CPU Utilization

Memory Usage

Network Throughput

MTTR
(Mean Time To Recovery)

MTBF
(Mean Time Between Failures)
```

---

# Interview Questions

## What is SRE?

```text
Applying Software Engineering
Practices To IT Operations
```

---

## What are Golden Signals?

```text
Latency
Traffic
Errors
Saturation
```

---

## Difference Between SLA and SLO?

```text
SLO = Target

SLA = Business Agreement
```

---

## What is Error Budget?

```text
Allowed Amount Of Failure
Before Reliability Degrades
```

---

# Summary

This guide covers:

✅ SRE Fundamentals

✅ SLI, SLO, SLA

✅ Error Budgets

✅ Incident Management

✅ Monitoring & Observability

✅ Logging & Tracing

✅ Runbooks

✅ Automation

✅ Kubernetes Reliability

✅ Disaster Recovery

✅ Capacity Planning

✅ High Availability

✅ Root Cause Analysis

✅ Production Support

✅ DevOps Integration

✅ Enterprise SRE Architecture

⭐ Keep this README as a complete Site Reliability Engineering (SRE) reference for SRE Engineers, DevOps Engineers, Cloud Engineers, Platform Engineers, System Administrators, and Production Support Teams.

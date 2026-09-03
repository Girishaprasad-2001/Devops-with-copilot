# Apache Kafka A to Z Commands, Architecture, Workflow & Configuration Guide

A complete Apache Kafka reference covering:

- Kafka Architecture
- Kafka Components
- Installation
- Configuration
- Producers
- Consumers
- Topics
- Partitions
- Replication
- Kafka Commands
- Kafka Administration
- Kubernetes Deployment
- DevOps Workflow
- Troubleshooting
- Production Best Practices

---

# What is Kafka?

Apache Kafka is a distributed event streaming platform used for:

- Real-time Data Streaming
- Event Processing
- Log Aggregation
- Messaging System
- Microservices Communication
- Data Pipelines
- Analytics

---

# Kafka Architecture

```text
+-------------+
| Producers   |
+------+------+
       |
       |
       ▼

+------------------+
|      Kafka       |
|     Brokers      |
+------------------+
       |
       |
       ▼

+------------------+
|    Consumers     |
+------------------+
```

---

# End-to-End Kafka Workflow

```text
Application
     |
     ▼

Producer

     |
     ▼

Kafka Topic

     |
     ▼

Partition

     |
     ▼

Kafka Broker

     |
     ▼

Consumer Group

     |
     ▼

Consumer

     |
     ▼

Database / API / Analytics
```

---

# Kafka Core Components

## Producer

Produces messages into Kafka Topics.

Example:

```text
Order Created
Payment Processed
User Registered
```

---

## Topic

Logical channel where messages are stored.

Example:

```text
orders
payments
users
logs
```

---

## Partition

Topics are divided into partitions.

```text
orders

Partition-0
Partition-1
Partition-2
```

Benefits:

- Scalability
- Parallel Processing

---

## Broker

Kafka Server.

Example:

```text
Broker-1
Broker-2
Broker-3
```

Default Port:

```text
9092
```

---

## Consumer

Reads messages from Kafka Topics.

---

## Consumer Group

Multiple consumers working together.

```text
Consumer Group

Consumer-1
Consumer-2
Consumer-3
```

---

# Kafka High-Level Architecture

```text
               +---------+
               |Producer |
               +----+----+
                    |
                    ▼

          +--------------------+
          |   Kafka Topic      |
          +--------------------+

                    |

         +----------+----------+

         ▼                     ▼

    Partition0          Partition1

         ▼                     ▼

       Broker1            Broker2

         ▼                     ▼

         +----------+----------+

                    ▼

             Consumer Group
```

---

# Prerequisites

```text
Java 17+
Linux Server
Minimum 4GB RAM
Port 9092 Open
Port 2181 Open (If Using ZooKeeper)
```

---

# Step 1: Install Java

```bash
sudo yum install java-17-openjdk -y
```

Verify:

```bash
java -version
```

---

# Step 2: Download Kafka

```bash
wget https://downloads.apache.org/kafka/3.8.0/kafka_2.13-3.8.0.tgz
```

---

# Extract Package

```bash
tar -xvzf kafka_2.13-3.8.0.tgz
```

---

# Navigate

```bash
cd kafka_2.13-3.8.0
```

---

# Kafka Directory Structure

```text
kafka/
├── bin
├── config
├── libs
├── logs
└── licenses
```

---

# Step 3: Start ZooKeeper (Legacy Mode)

```bash
bin/zookeeper-server-start.sh \
config/zookeeper.properties
```

Run Background:

```bash
nohup bin/zookeeper-server-start.sh \
config/zookeeper.properties &
```

Default Port:

```text
2181
```

---

# Step 4: Start Kafka Broker

```bash
bin/kafka-server-start.sh \
config/server.properties
```

Background:

```bash
nohup bin/kafka-server-start.sh \
config/server.properties &
```

Default Port:

```text
9092
```

---

# Verify Kafka

```bash
netstat -tulpn | grep 9092
```

or

```bash
ss -tulnp | grep 9092
```

---

# Kafka Topic Commands

---

## Create Topic

```bash
bin/kafka-topics.sh \
--create \
--topic orders \
--bootstrap-server localhost:9092 \
--partitions 3 \
--replication-factor 1
```

---

## List Topics

```bash
bin/kafka-topics.sh \
--list \
--bootstrap-server localhost:9092
```

---

## Describe Topic

```bash
bin/kafka-topics.sh \
--describe \
--topic orders \
--bootstrap-server localhost:9092
```

---

## Delete Topic

```bash
bin/kafka-topics.sh \
--delete \
--topic orders \
--bootstrap-server localhost:9092
```

---

# Producer Commands

## Start Producer

```bash
bin/kafka-console-producer.sh \
--topic orders \
--bootstrap-server localhost:9092
```

Example:

```text
Order1
Order2
Order3
```

---

# Consumer Commands

## Start Consumer

```bash
bin/kafka-console-consumer.sh \
--topic orders \
--bootstrap-server localhost:9092
```

---

## Read From Beginning

```bash
bin/kafka-console-consumer.sh \
--topic orders \
--from-beginning \
--bootstrap-server localhost:9092
```

---

# Consumer Group Commands

## List Consumer Groups

```bash
bin/kafka-consumer-groups.sh \
--bootstrap-server localhost:9092 \
--list
```

---

## Describe Group

```bash
bin/kafka-consumer-groups.sh \
--bootstrap-server localhost:9092 \
--group my-group \
--describe
```

---

# Partition Commands

## Increase Partitions

```bash
bin/kafka-topics.sh \
--alter \
--topic orders \
--partitions 5 \
--bootstrap-server localhost:9092
```

---

# Kafka Broker Configuration

File:

```bash
config/server.properties
```

Important Settings:

```properties
broker.id=1

listeners=PLAINTEXT://0.0.0.0:9092

log.dirs=/tmp/kafka-logs

num.partitions=3

default.replication.factor=1
```

---

# Retention Configuration

Message Retention:

```properties
log.retention.hours=168
```

7 Days

Retention Size:

```properties
log.retention.bytes=1073741824
```

1 GB

---

# Kafka Replication

```text
Topic
 |
 +-- Partition-0
       |
       +-- Leader (Broker1)
       |
       +-- Follower (Broker2)

```

Benefits:

- High Availability
- Fault Tolerance

---

# Kafka Data Flow

```text
Producer
    |
    ▼

Kafka Topic

    |
    ▼

Partition

    |
    ▼

Broker

    |
    ▼

Consumer Group

    |
    ▼

Consumer
```

---

# Kafka Performance Commands

## Broker API Versions

```bash
bin/kafka-broker-api-versions.sh \
--bootstrap-server localhost:9092
```

---

## Check Consumer Lag

```bash
bin/kafka-consumer-groups.sh \
--bootstrap-server localhost:9092 \
--group my-group \
--describe
```

---

# Kafka Logs

Location:

```bash
logs/
```

Server Logs:

```bash
logs/server.log
```

View:

```bash
tail -f logs/server.log
```

---

# Kafka Security

## SSL

```properties
security.protocol=SSL
```

---

## SASL

```properties
security.protocol=SASL_SSL
```

---

# Kafka Kubernetes Deployment

---

## Zookeeper Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: zookeeper
spec:
  replicas: 1
  selector:
    matchLabels:
      app: zookeeper
  template:
    metadata:
      labels:
        app: zookeeper
    spec:
      containers:
      - name: zookeeper
        image: confluentinc/cp-zookeeper
```

---

## Kafka Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kafka
spec:
  replicas: 1
  selector:
    matchLabels:
      app: kafka
  template:
    metadata:
      labels:
        app: kafka
    spec:
      containers:
      - name: kafka
        image: confluentinc/cp-kafka
```

Deploy:

```bash
kubectl apply -f kafka.yaml
```

---

# DevOps Workflow

```text
Application
      |
      ▼

Producer

      |
      ▼

Kafka

      |
      ▼

Consumer

      |
      ▼

Spring Boot

      |
      ▼

Database

      |
      ▼

Grafana / Analytics
```

---

# Kafka Monitoring

Tools:

```text
Prometheus
Grafana
Kafka Exporter
Confluent Control Center
```

Metrics:

```text
Broker Health
Topic Throughput
Consumer Lag
Partitions
Replication Status
```

---

# Daily Kafka Commands

```bash
kafka-topics.sh --list

kafka-topics.sh --describe

kafka-console-producer.sh

kafka-console-consumer.sh

kafka-consumer-groups.sh --list

kafka-consumer-groups.sh --describe

kafka-server-start.sh

kafka-server-stop.sh

zookeeper-server-start.sh

zookeeper-server-stop.sh
```

---

# Troubleshooting Commands

## Running Processes

```bash
ps -ef | grep kafka
```

## Kafka Port

```bash
ss -tulnp | grep 9092
```

## ZooKeeper Port

```bash
ss -tulnp | grep 2181
```

## Kafka Logs

```bash
tail -f logs/server.log
```

## Consumer Lag

```bash
kafka-consumer-groups.sh \
--bootstrap-server localhost:9092 \
--describe \
--group my-group
```

---

# Common Interview Workflow

```text
Producer
   |
   ▼
Topic
   |
   ▼
Partition
   |
   ▼
Broker
   |
   ▼
Consumer Group
   |
   ▼
Consumer
   |
   ▼
Database
```

---

# Summary

This guide covers:

- Kafka Installation
- ZooKeeper
- Kafka Broker
- Topics
- Producers
- Consumers
- Consumer Groups
- Partitions
- Replication
- Retention Policies
- Security
- Kubernetes Deployment
- Monitoring
- Troubleshooting
- Production Best Practices

⭐ Keep this README as a complete Apache Kafka reference for DevOps, SRE, Platform Engineering, Microservices, Event Streaming, and Production 

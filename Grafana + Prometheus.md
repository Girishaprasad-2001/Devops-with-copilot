# Grafana + Prometheus + Node Exporter Setup & Monitoring Flow

A complete step-by-step guide for installing and configuring:

- Node Exporter
- Prometheus
- Grafana

This setup is widely used for:

- Linux Server Monitoring
- DevOps Monitoring
- SRE Operations
- Kubernetes Monitoring
- Infrastructure Observability
- Production Support

---

# Architecture Overview

```text
+----------------------+
| Linux Server         |
| CPU/MEM/DISK/NET     |
+----------+-----------+
           |
           | Exposes Metrics
           |
           v
+----------------------+
| Node Exporter        |
| Port : 9100          |
+----------+-----------+
           |
           | Scrape Metrics
           |
           v
+----------------------+
| Prometheus           |
| Port : 9090          |
+----------+-----------+
           |
           | Query Metrics
           |
           v
+----------------------+
| Grafana              |
| Port : 3000          |
+----------------------+
```

---

# Monitoring Workflow

```text
Linux Server
     |
     ▼
Node Exporter
     |
     ▼
Prometheus
     |
     ▼
Grafana Dashboard
     |
     ▼
Email / Slack Alerts
```

---

# Component Overview

## Node Exporter

Collects Linux server metrics:

- CPU Usage
- Memory Usage
- Disk Usage
- Filesystem
- Network
- Load Average
- Process Information

Default Port:

```text
9100
```

---

## Prometheus

Responsible for:

- Collecting Metrics
- Time-Series Storage
- PromQL Queries
- Alert Rules

Default Port:

```text
9090
```

---

## Grafana

Responsible for:

- Dashboard Creation
- Visualization
- Reporting
- Alerting

Default Port:

```text
3000
```

---

# Prerequisites

Server Requirements:

```text
Ubuntu 22.04 / RHEL 8+
CPU   : 2 Core
RAM   : 4 GB
Disk  : 20 GB
```

Required Ports:

```text
3000  Grafana
9090  Prometheus
9100  Node Exporter
```

---

# Step 1: Install Node Exporter

## Create User

```bash
sudo useradd \
--no-create-home \
--shell /bin/false \
node_exporter
```

---

## Download Node Exporter

```bash
cd /tmp
```

```bash
wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-1.8.2.linux-amd64.tar.gz
```

---

## Extract Package

```bash
tar -xvf node_exporter-*.tar.gz
```

---

*# Copy Binary

```bash
sudo cp*node_exporter-*/node_exporter \
/usr/local/bin/
```

---

## Set Permissions

```bash
sudo chown node_exporter:node_exporter \
/usr/local/bin/node_exporter
```

---

## Create Service

```bash
sudo vi /etc/systemd/system/node_exporter.service
```

```ini
[Unit]
Description=Node Exporter

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

---

## Enable Service

```bash
sudo systemctl daemon-reload
```

```bash
sudo systemctl enable node_exporter
```

```bash
sudo systemctl start node_exporter
```

---

## Verify Service

```bash
sudo systemctl status node_exporter
```

---

## Verify Metrics

```bash
curl localhost:9100/metrics
```

Expected Output:

```text
node_memory_MemFree_bytes
node_cpu_seconds_total
node_filesystem_size_bytes
```

---

# Step 2: Install Prometheus

## Create User

```bash
sudo useradd \
--no-create-home \
--shell /bin/false \
prometheus
```

---

## Create Directories

```bash
sudo mkdir /etc/prometheus
```

```bash
sudo mkdir /var/lib/prometheus
```

---

## Download Prometheus

```bash
cd /tmp
```

```bash
wget https://github.com/prometheus/prometheus/releases/latest/download/prometheus-2.55.0.linux-amd64.tar.gz
```

---

## Extract Package

```bash
tar -xvf prometheus-*.tar.gz
```

---

## Copy Files

`*`bash
sudo cp prometheus-*/prometh*us \
/usr/local/bin/
```

```bash
*udo cp prometheus-*/promtool \
/us*/local/bin/
```

```bash
sudo cp p*ometheus-*/prometheus.yml \
/etc/p*ometheus/
```

---

## Configure P*ometheus

```bash
sudo vi /etc/pro*etheus/prometheus.yml
```

```yaml*global:
  scrape_interval: 15s

sc*ape_configs:

  - job_name: promet*eus

    static_configs:
      - t*rgets:
          - localhost:9090
*  - job_name: node_exporter

    s*atic_configs:
      - targets:
   *      - localhost:9100
```

---

#* Create Service

```bash
sudo vi /*tc/systemd/system/prometheus.servi*e
```

```ini
[Unit]
Description=P*ometheus

[Service]
User=prometheu*

ExecStart=/usr/local/bin/prometh*us \
 --config.file=/etc/prometheu*/prometheus.yml \
 --storage.tsdb.*ath=/var/lib/prometheus

[Install]*WantedBy=multi-user.target
```

--*

## Start Service

```bash
sudo s*stemctl daemon-reload
```

```bash*sudo systemctl enable prometheus
`*`

```bash
sudo systemctl start pr*metheus
```

---

## Verify

```ba*h
sudo systemctl status prometheus*```

---

Open Browser:

```text
http://SERVER-IP:9090
```

Health Ch*ck:

```bash
curl localhost:9090/-*healthy
```

---

# Step 3: Instal* Grafana

## Ubuntu

```bash
sudo *pt update
```

```bash
sudo apt in*tall -y grafana
```

---

## RHEL/*ocky Linux

```bash
sudo yum insta*l grafana -y
```

---

## Enable S*rvice

```bash
sudo systemctl enab*e grafana-server
```

```bash
sudo*systemctl start grafana-server
```*
---

## Verify

```bash
sudo syst*mctl status grafana-server
```

--*

Open Browser:

```text
http://SERVER-IP:3000
```

Default Credentia*s:

```text
Username : admin

Pass*ord : admin
```

---

# Step 4: Co*figure Grafana

Navigate:

```text*Home
  ↓
Connections
  ↓
Data Sour*es
  ↓
Add Data Source
```

Choose*

```text
Prometheus
```

URL:

``*text
http://localhost:9090
```

Cl*ck:

```text
Save & Test
```

Expe*ted Message:

```text
Data source *s working
```

---

# Step 5: Impo*t Node Exporter Dashboard

Navigat*:

```text
Grafana
  ↓
Dashboards
* ↓
Import
```

Dashboard ID:

```t*xt
1860
```

Select:

```text
Prom*theus Data Source
```

Click:

```*ext
Import
```

---

# Step 6: Ver*fy Metrics

Open Prometheus:

```t*xt
http://SERVER-IP:9090
```

Chec* Targets:

```text
Status
     ↓
T*rgets
```

Expected:

```text
UP
`*`

for:

```text
prometheus
node_e*porter
```

---

# Common PromQL Q*

# 🚀 Real-Time Infrastructure Monitoring & Alerting using Prometheus & Grafana on EC2

---

## 📌 Project Overview

This project implements a **real-time infrastructure monitoring and alerting system** using Prometheus and Grafana on AWS EC2.

Previously, server health (CPU, memory, disk) was monitored manually using SSH. This solution automates monitoring by providing **live dashboards and proactive alerts**, helping detect issues before they impact system availability.

---

## 🎯 Objective

- Monitor multiple EC2 instances in real time  
- Collect and store system metrics centrally  
- Visualize infrastructure health using dashboards  
- Trigger alerts when thresholds are exceeded  
- Enable email notifications for critical alerts  

---

## 🏗️ Architecture & Workflow

1. Node Exporter runs on application servers and exposes system metrics  
2. Prometheus (monitoring server) scrapes metrics from all servers  
3. Grafana connects to Prometheus to visualize data  
4. Alert rules evaluate metrics continuously  
5. Email notifications are sent when alerts are triggered  

---

## ⚙️ Tech Stack

- AWS EC2  
- Prometheus  
- Grafana  
- Node Exporter  

---

## 🖥️ Infrastructure Setup

### 🔹 Monitoring Server (1 EC2)
- Installed:
  - Prometheus  
  - Grafana  

### 🔹 Application Servers (2 EC2)
- Installed:
  - Node Exporter  

---

## 📊 Prometheus Configuration

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"
    static_configs:
      - targets:
          - "<APP_SERVER_1_IP>:9100"
          - "<APP_SERVER_2_IP>:9100"
```
## 📊 Grafana Dashboard

- Added Prometheus as a data source  
- Imported Node Exporter Full Dashboard (ID: 1860)  
- Created panels for:
  - CPU Usage  
  - Memory Utilization  
  - Disk Usage  


## 🚨 Alerting

- Configured alert rule:
  - Condition: CPU usage > 70%  
  - Evaluation interval: 1 minute  
  - Pending duration: 1 minute  

### 🔔 Email Notification Setup

- Configured SMTP in Grafana using Gmail  
- Used App Password for authentication  
- Enabled email alerts for notifications  


### 🔧 Alert Query (PromQL)

```promql
100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[1m])) * 100)
```

### 🧪 Testing

Simulated high CPU load:
```
for i in {1..10}; do yes > /dev/null & done
```
Stopped load:
```
killall yes
```
### 🔥 Key Learnings
Built a complete monitoring system from scratch
Learned real-time infrastructure monitoring concepts
Gained hands-on experience with PromQL queries
Configured alerting and notification systems
Debugged real-world monitoring issues

### 🎯 Outcome

Successfully created a system that:

Replaces manual server checks
Provides real-time visibility
Detects issues proactively
Sends automated email alerts
Improves system reliability

### 📄 Summary

This project demonstrates the implementation of a real-time infrastructure monitoring and alerting system using Prometheus and Grafana on AWS EC2. System metrics such as CPU, memory, and disk usage are collected from multiple servers and visualized through interactive dashboards.

A threshold-based alerting mechanism is configured to detect high CPU usage and trigger automated email notifications, enabling proactive issue detection. This solution eliminates manual monitoring, improves system visibility, and enhances overall reliability.

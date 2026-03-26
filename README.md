#  Real-Time Infrastructure Monitoring & Alerting using Prometheus & Grafana

---

## ➤ Project Overview

This project implements a **real-time infrastructure monitoring and alerting system** using Prometheus and Grafana on AWS EC2.

Previously, server health (CPU, memory, disk) was monitored manually using SSH. This solution automates monitoring by providing **live dashboards and proactive alerts**, helping detect issues before they impact system availability.

---

## ➤ Objective

- Monitor multiple EC2 instances in real time  
- Collect and store system metrics centrally  
- Visualize infrastructure health using dashboards  
- Trigger alerts when thresholds are exceeded  
- Enable email notifications for critical alerts  

---

## ➤ Architecture Diagram
![Architecture](./pictures/architecture.png)

---

## ➤ Architecture & Workflow

1. Node Exporter runs on application servers and exposes system metrics  
2. Prometheus (monitoring server) scrapes metrics from all servers  
3. Grafana connects to Prometheus to visualize data  
4. Alert rules evaluate metrics continuously  
5. Email notifications are sent when alerts are triggered  

---

## ➤ Tech Stack

- AWS EC2  
- Prometheus  
- Grafana  
- Node Exporter  

---

## ➤ Infrastructure Setup

### 🔹 Monitoring Server (1 EC2)
- Installed:
  - Prometheus
  ![prometheus](./pictures/prometheus.png)  
  - Grafana  
  ![grafana](./pictures/grafana.png)

### 🔹 Application Servers (2 EC2)
- Installed:
  - Node Exporter 
  ![node exporter app 1](./pictures/node-exporter-app1.png)
  ![node exporter app 2](./pictures/node-exporter-app2.png)

### Instances for this project
![ec2](./pictures/ec2.png)

---

## ➤ Prometheus Configuration
Configure Prometheus to Scrape Node Exporter
- Edit configuration:
```
sudo vim /etc/prometheus/prometheus.yml
```


```yaml
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
![prometheus file](./pictures/prometheus-file.png)

- Restart Prometheus:
```
sudo systemctl restart prometheus
```

---

## ➤ Grafana Dashboard

- Added Prometheus as a data source  
- Imported Node Exporter Full Dashboard (ID: 1860)  
  ![import-grafana-dash](./pictures/import-grafana-dash.png)
- Grafana dashboard 
  ![Grafana dash](./pictures/grafana-dash.png)

---

## ➤ Alerting

- Configured alert rule:
  - Condition: CPU usage > 70%  
  - Evaluation interval: 1 minute  
  - Pending duration: 1 minute 
  - Add Alert Query (PromQL)

    ```promql
    100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[1m])) * 100)
    ```
 
  ![alert-rule](./pictures/alert-rule.png)

## ➤ Email Notification Setup

- Configured SMTP in Grafana using Gmail 
  - Edit file:
    ```
    sudo vim /etc/grafana/grafana.ini
    ``` 
  - Find [smtp] section and update:
    ```
    [smtp]
    enabled = true
    host = smtp.gmail.com:587
    user = your-email@gmail.com
    password = your-app-password
    from_address = your-email@gmail.com
    from_name = Grafana
    skip_verify = true
    ```
    ![SMTP in Grafana](./pictures/email-configure.png)

- Used App Password from your gmail for authentication 
- Restart Grafana
  ```
  sudo systemctl restart grafana-server
  ```

- Enabled email alerts for notifications  

- Select contact-point 
  ![contact-point](./pictures/contact-point.png)

- Alert Rule is Configured now

![alert-dash](./pictures/alert-dash.png)



## ➤ Testing

- Simulated high CPU load:
  ```
  for i in {1..10}; do yes > /dev/null & done
  ```
  ![increase cpu load](./pictures/increase%20cpu.png)

- Check Grafana Dashboard to see increasing cpu load
  ![high-cpu](./pictures/high-cpu.png)

- Check Alert Dashboard to see Firing alarm
  ![alert-firing](./pictures/alert-firing.png)

- Check Email Notifications

  - Mail for CPU high usage alert
    ![mail-1](./pictures/mail-1.png)


- Stopped load:
  ```
  killall yes
  ```
  ![decrease-cpu](./pictures/decrease-cpu.png)

- Check Grafana Dashboard to see increasing cpu load
  ![low-cpu](./pictures/low-cpu.png)

- Check Alert Dashboard to see normal state of instance
  ![alert-firing](./pictures/normal-state.png)
 
- Check Email Notifications
  - Mail for CPU usage comes below threshold
    ![mail-2](./pictures/mail-2.png)

---

## ➤ Key Learnings
Built a complete monitoring system from scratch
Learned real-time infrastructure monitoring concepts
Gained hands-on experience with PromQL queries
Configured alerting and notification systems
Debugged real-world monitoring issues

---

## ➤ Outcome

Successfully created a system that:

Replaces manual server checks
Provides real-time visibility
Detects issues proactively
Sends automated email alerts
Improves system reliability

---

## ➤ Summary

This project demonstrates the implementation of a real-time infrastructure monitoring and alerting system using Prometheus and Grafana on AWS EC2. System metrics such as CPU, memory, and disk usage are collected from multiple servers and visualized through interactive dashboards.

A threshold-based alerting mechanism is configured to detect high CPU usage and trigger automated email notifications, enabling proactive issue detection. This solution eliminates manual monitoring, improves system visibility, and enhances overall reliability.

---

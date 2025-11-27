# 🚀 Observability Stack — Prometheus | Grafana | Node Exporter | cAdvisor | Docker Compose

<p align="center">
  <img src="https://img.shields.io/badge/Prometheus-Monitoring-orange?logo=prometheus&logoColor=white" />
  <img src="https://img.shields.io/badge/Grafana-Dashboards-F47A20?logo=grafana&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Node%20Exporter-System%20Metrics-557C94?logo=linux&logoColor=white" />
  <img src="https://img.shields.io/badge/cAdvisor-Container%20Metrics-1E88E5?logo=google&logoColor=white" />
</p>

---

# 📘 Project Overview

This repository contains a **complete Observability & Monitoring Stack** built using:

* **Prometheus** – Time-series DB + Metrics Scraping
* **Grafana** – Dashboards + Visualization
* **Node Exporter** – System metrics
* **cAdvisor** – Container metrics
* **Notes App** – Sample application with custom metrics
* **Docker Compose** – Orchestration

Ideal for:

* DevOps labs
* Monitoring POCs
* Cloud engineers
* SRE training environments

---

# 📑 Table of Contents

1. Architecture
2. Features
3. Repository Structure
4. Prerequisites
5. Installation
6. Configuration
7. Dashboards
8. Troubleshooting
9. Contributing
10. License

---

# 🏗 Architecture

```
                           Grafana (Dashboards)
                                   |
                                   |
                          Prometheus (Scraper)
                ----------------------------------------
                 |                 |                 |
          Node Exporter       cAdvisor           Notes App
         (System Metrics)  (Container Metrics)  (App Metrics)
```

---

# ✨ Features

| Feature                | Description                                             |
| ---------------------- | ------------------------------------------------------- |
| 🔍 Metrics Collection  | Collect system, container & application metrics         |
| 📊 Dashboards          | Pre-built Grafana dashboards (Node Exporter + cAdvisor) |
| ⚡ Real-time Monitoring | Auto-refresh, time-series visualization                 |
| 🧩 Modular Design      | Add or remove services easily                           |
| 🐳 100% Containerized  | Works everywhere using Docker Compose                   |
| 🔌 Custom App Metrics  | Example Notes App included                              |

---

# 📂 Repository Structure

```
observability/
│
├── docker-compose.yml
├── prometheus.yml
│
├── data/
│   ├── grafana/
│   │   └── provisioning/
│   │       ├── datasources/
│   │       └── dashboards/
│
└── README.md
```

---

# 🛠 Prerequisites

| Component             | Version            |
| --------------------- | ------------------ |
| Ubuntu / Linux        | 20.04+             |
| Docker                | 20+                |
| Docker Compose Plugin | v2+                |
| RAM                   | 2–4 GB recommended |

---

# 🚀 Installation & Setup

## 1. Clone Repository

```
git clone https://github.com/<username>/observability.git
cd observability
```

## 2. Start the Monitoring Stack

```
docker compose up -d
docker ps
```

## 3. Access Web Interfaces

| Service       | URL                                                            |
| ------------- | -------------------------------------------------------------- |
| Grafana       | [http://localhost:3000](http://localhost:3000)                 |
| Prometheus    | [http://localhost:9090](http://localhost:9090)                 |
| cAdvisor      | [http://localhost:8080](http://localhost:8080)                 |
| Node Exporter | [http://localhost:9100/metrics](http://localhost:9100/metrics) |
| Notes App     | [http://localhost:8000](http://localhost:8000)                 |

---

# ⚙️ Prometheus Configuration

```
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "Docker"
    static_configs:
      - targets: ["cadvisor:8080"]

  - job_name: "NodeExporter"
    static_configs:
      - targets: ["node-exporter:9100"]

  - job_name: "NotesApp"
    static_configs:
      - targets: ["notes-app:8000"]
```

---

# 📊 Grafana Dashboards

### Import Recommended Dashboards:

| Dashboard          | ID       |
| ------------------ | -------- |
| Node Exporter Full | **1860** |
| cAdvisor Metrics   | **893**  |

### Setup Steps:

1. Login to Grafana (`admin / admin`)
2. Add Prometheus Data Source
3. Import dashboard → Enter ID

---

# 🐳 Docker Compose Setup

```
version: "3.8"

networks:
  monitoring:
    driver: bridge

volumes:
  prometheus_data: {}
  grafana_data: {}

services:
  grafana:
    image: grafana/grafana-enterprise
    container_name: grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana_data:/var/lib/grafana
      - ./data/grafana/provisioning/datasources:/etc/grafana/provisioning/datasources
      - ./data/grafana/provisioning/dashboards:/etc/grafana/provisioning/dashboards
    networks:
      - monitoring

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - prometheus_data:/prometheus
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro
    networks:
      - monitoring

  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    restart: unless-stopped
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    command:
      - '--path.procfs=/host/proc'
      - '--path.rootfs=/rootfs'
      - '--path.sysfs=/host/sys'
      - '--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)'
    expose:
      - 9100
    networks:
      - monitoring

  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    container_name: cadvisor
    ports:
      - "8080:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:rw
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
    networks:
      - monitoring

  notes-app:
    image: satyamsri/notes-app
    container_name: notes-app
    ports:
      - "8000:8000"
    networks:
      - monitoring
```

---

# 🆘 Troubleshooting

* Grafana not loading?

```
docker logs grafana
```

* Prometheus cannot reach exporters? Verify service names.
* Node Exporter metrics empty? Check mounts:

```
/proc
/sys
/
```

---

# 🤝 Contributing

1. Fork repository
2. Create feature branch
3. Commit changes
4. Create pull request

---

# 📜 License

This project is licensed under the **MIT License**.

---

# ⭐ Support

If this project helped you, please ⭐ the repository!

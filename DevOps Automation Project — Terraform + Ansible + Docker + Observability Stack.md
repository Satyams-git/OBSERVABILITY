# 🚀 Complete DevOps Automation Project — Terraform + Ansible + Docker + Observability Stack

A fully automated infrastructure + configuration + deployment workflow using Terraform (IaC), Ansible (CM), Docker (runtime), and a complete Observability Stack.

This canvas is **copy-ready** and can be downloaded directly.

---

# 📘 Project Overview

This project provisions infrastructure using **Terraform**, configures servers using **Ansible**, builds Docker images from source, pushes them to Docker Hub, deploys applications on worker nodes, and installs a full **Observability Stack** on both servers.

Applications deployed:

* **Worker-1:** Notes App
  Repository: [https://github.com/Satyams-git/notes-app.git](https://github.com/Satyams-git/notes-app.git)

* **Worker-2:** Nginx Frontend App
  Repository: [https://github.com/Satyams-git/nginx-frontend-app.git](https://github.com/Satyams-git/nginx-frontend-app.git)

Observability Setup:

* Prometheus
* Grafana
* Node Exporter
* cAdvisor

---

# 🏗 High-Level Architecture

```
                           Terraform
                         (Provisioning)
                                |
                                v
     -----------------------------------------------------
     |                        |                           |
Control Node           Worker Node 1               Worker Node 2
(Ansible Host)         Notes App (Docker)          Nginx App (Docker)
                           |                           |
                           ------- Monitoring Stack -----
                                   Prometheus
                                   Grafana
                                   Node Exporter
                                   cAdvisor
```

---

# 🧰 Tools & Technologies

* Terraform
* Ansible
* Docker & Docker Compose
* Prometheus + Grafana
* Node Exporter & cAdvisor
* GitHub Source Code

---

# ⚙️ Terraform Tasks — Infrastructure Provisioning

Terraform creates:

* **1 Control Node (Ansible host)**
* **2 Worker Nodes** (apps deployed)

### Commands:

```
terraform init
terraform validate
terraform apply -auto-approve
```

Terraform Outputs:

* control_node_ip
* worker_1_ip
* worker_2_ip

These IPs are inserted into Ansible inventory.

---

# ⚙️ Ansible Tasks — Application Build, Push & Deployment

Once Terraform provisions servers, Ansible will perform:

## 1. Clone Repositories

```
git clone https://github.com/Satyams-git/notes-app.git
git clone https://github.com/Satyams-git/nginx-frontend-app.git
```

## 2. Build Docker Images (on Control Node)

### Notes App

```
docker build -t <dockerhub_username>/notes-app:latest ./notes-app
```

### Nginx Frontend App

```
docker build -t <dockerhub_username>/nginx-frontend:latest ./nginx-frontend-app
```

## 3. Push Images to Docker Hub

```
docker login -u <dockerhub_username> -p <password>
docker push <dockerhub_username>/notes-app:latest
docker push <dockerhub_username>/nginx-frontend:latest
```

---

# 📦 Worker Deployments Using Docker Compose

## Worker-1 (Notes App)

**docker-compose.yml:**

```
version: "3.8"
services:
  notes-app:
    image: <dockerhub_username>/notes-app:latest
    ports:
      - "8000:8000"
```

```
docker compose up -d
```

## Worker-2 (Nginx Frontend App)

**docker-compose.yml:**

```
version: "3.8"
services:
  nginx-app:
    image: <dockerhub_username>/nginx-frontend:latest
    ports:
      - "80:80"
```

```
docker compose up -d
```

---

# 📡 Observability Stack Deployment (Ansible)

Ansible will install monitoring tools on both worker nodes.

## 1. Install Node Exporter

* Download binary
* Create systemd service
* Enable + start

## 2. Install cAdvisor

* Run via Docker

## 3. Install Prometheus

```
prometheus.yml
scrape_configs:
  - job_name: "NodeExporter"
    static_configs:
      - targets: ["worker1:9100", "worker2:9100"]

  - job_name: "cAdvisor"
    static_configs:
      - targets: ["worker1:8080", "worker2:8080"]
```

## 4. Install Grafana

* Add Prometheus as data source
* Import dashboards: **1860 (Node Exporter)** + **893 (cAdvisor)**

---

# 📁 Recommended Directory Structure

```
project/
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── provider.tf
│
├── ansible/
│   ├── inventory.ini
│   ├── playbooks/
│   │   ├── docker-install.yml
│   │   ├── build-push.yml
│   │   ├── deploy-notes.yml
│   │   ├── deploy-nginx.yml
│   │   ├── observability.yml
│   │   └── site.yml
│   └── roles/
│       ├── docker/
│       ├── node_exporter/
│       ├── cadvisor/
│       ├── grafana/
│       └── prometheus/
│
└── README.md
```

---

# 🔁 End-to-End Execution Flow

### **Step 1 — Create Infrastructure**

```
cd terraform
terraform apply -auto-approve
```

### **Step 2 — Update Ansible Inventory with Terraform Outputs**

```
[control]
control ansible_host=<IP>

[workers]
worker1 ansible_host=<IP>
worker2 ansible_host=<IP>
```

### **Step 3 — Run Full Automation**

```
cd ansible
ansible-playbook site.yml
```

---

# ⭐ This is your complete downloadable canvas.

Let me know if you want:

* Terraform code
* Full Ansible playbooks
* Architecture diagram (PNG)
* CI/CD workflow
* GitHub Actions pipeline

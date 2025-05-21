# 🚀 Kubernetes Monitoring & Logging Dashboard with Grafana, Prometheus, and Loki

## 📘 Assignment Objective

Create a real-time monitoring and logging solution using Prometheus and Grafana, deployed on a Minikube cluster running inside an AWS EC2 instance. Logs are collected via Loki and Promtail, and all system metrics are visualized through Grafana dashboards.

---

## ✅ Environment Setup

### 🔹 EC2 Instance

* **OS:** Ubuntu 22.04 LTS
* **Instance Type:** t3.large or higher
* **Disk:** 20 GB minimum
* **Ports Opened (Security Group):**

  * TCP: `22`, `30000-32767`, `9090`, `3000`, `3100`

---

## ⚙️ Step 1: Install Dependencies

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget apt-transport-https ca-certificates gnupg docker.io conntrack
```

---

## ⚙️ Step 2: Install Minikube & kubectl

```bash
# Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# kubectl
KUBECTL_VERSION=$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)
curl -LO https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

---

## ⚙️ Step 3: Start Minikube Cluster

```bash
sudo usermod -aG docker $USER
newgrp docker

minikube start --driver=docker --memory=4096mb --cpus=2
kubectl get nodes
```

---

## ⚙️ Step 4: Deploy Sample Application

```bash
kubectl create namespace application
kubectl create deployment nginx --image=nginx -n application
kubectl expose deployment nginx --port=80 --type=NodePort -n application
minikube service nginx -n application --url
```

---

## 📊 Step 5: Monitoring Setup with Prometheus & Grafana

### Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
kubectl create namespace monitoring
```

### Install Prometheus

```bash
helm install prometheus prometheus-community/prometheus --namespace monitoring
```

### Install Grafana

```bash
helm install grafana grafana/grafana \
  --namespace monitoring \
  --set adminPassword='admin' \
  --set image.tag="10.2.3" \
  --set service.type=NodePort
```

### Access Grafana

```bash
kubectl get svc -n monitoring
curl ifconfig.me
```

Open in browser:

```
http://<EC2_PUBLIC_IP>:<NodePort>
```

Login:

* Username: `admin`
* Password: `admin`

---

## 📈 Add Prometheus Data Source in Grafana

1. Go to ⚙️ → Data Sources → Add Data Source
2. Select **Prometheus**
3. URL: `http://prometheus-server.monitoring.svc.cluster.local:80`
4. Click **Save & Test**

---

## 📌 Next Steps

* Create Grafana dashboards for:

  * CPU usage
  * Memory usage
  * Node/pod availability
  * Usage trends over time
* Deploy Loki & Promtail for log monitoring
* Create LogQL queries for app logs

---

## 📷 Screenshots to Include in Report

* EC2 instance config
* `kubectl get pods -n monitoring`
* Grafana dashboards
* Loki log panel
* Prometheus metrics
* Minikube service URLs

---

## 📄 Deliverables

* PDF report with:

  * Step-by-step procedure
  * Dashboard screenshots
  * Public dashboard URLs (if available)
  * Challenges faced & solutions
* (Optional) GitHub repo with `README.md`, `helm-values.yaml`, dashboard JSON

---

## 😋 Author

**Name:** *Your Name*
**Institution:** *Your Institute*
**Assignment:** Module 7 - Kubernetes Monitoring & Logging

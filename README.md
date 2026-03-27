# Spring Boot + MySQL Application + Kube Deployment
---
<!-- <img width="1132" height="774" alt="image" src="https://github.com/user-attachments/assets/461d49db-abe8-476c-9dad-544b8a285d41" /> -->

---
## Configuration Details
### `application.properties`
```properties
spring.datasource.url=jdbc:mysql://mysql:3306/myapplication?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=1234
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```
---
## admin login
- username: admin
- password: admin
---
## nginx configuration
---
### Create Nginx config: On the Nginx server:
```bash
sudo vi /etc/nginx/sites-available/reverse-proxy
```
- Paste this:
```bash
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;

    location / {
        proxy_pass http://13.235.57.138:8081;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
### Enable config
```bash
sudo rm /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
---




---

# 🚀 Spring Boot + MySQL Deployment using Kubernetes (kubeadm)

## 📌 Overview

This project demonstrates deployment of a **Spring Boot application with MySQL** using:

* Docker (Containerization)
* Kubernetes (kubeadm cluster)
* Multi-node setup (Master + Worker)
* Docker Hub (Image registry)

---

## 🧱 Architecture

```
Master Node (Control Plane)
        ↓
Worker Node (Runs Pods)
        ↓
Docker Server (Build & Push Images)
```

---

## ⚙️ Prerequisites

* Ubuntu/Linux (3 systems recommended)
* Minimum:

  * 2 CPU
  * 4–8 GB RAM
* Docker Hub Account
* Internet connectivity

---

## 🖥️ Master Node Setup

```bash
apt update

# Install Kubernetes Master
curl -sL https://tinyurl.com/mt346aen | bash

# Install dependencies
apt install openjdk-17-jre-headless -y
apt install maven -y

# Clone repository
git clone https://github.com/sakit333/spring_mysql_kubeadm_sakcoorg.git
cd spring_mysql_kubeadm_sakcoorg

# Edit scripts if required
vi docker_build_push.sh
vi k8s-deploy.sh   # ensure ./kubescripts path is correct

# Deploy application
bash k8s-deploy.sh
```

---

## 👷 Worker Node Setup

```bash
apt update

# Install Kubernetes Worker
curl -sL https://tinyurl.com/yvmvxmzb | bash

# Join cluster (update if token changes)
sudo kubeadm join 172.31.40.37:6443 \
--token v96oir.ifksg4mot37dclz9 \
--discovery-token-ca-cert-hash sha256:eef265a455bcbb05369a52e2754393fac139e9dbb948150b1e2e4aa0e9e2946a
```

Verify:

```bash
kubectl get nodes
```

---

## 🐳 Docker Server Setup

```bash
apt update

# Install Docker
apt install docker.io -y
docker --version

# Install Java & Maven
apt install openjdk-17-jre-headless -y
apt install maven -y

# Clone project
git clone https://github.com/sakit333/spring_mysql_kubeadm_sakcoorg.git
cd spring_mysql_kubeadm_sakcoorg

# Build Docker image
docker build -t spring_mysql_kubeadm .

# Login to Docker Hub
docker login -u thanushrii

# Tag image
docker tag spring_mysql_kubeadm thanushrii/spring_mysql_kubeadm

# Push image
docker push thanushrii/spring_mysql_kubeadm
```

---

## 📦 Kubernetes Deployment

Ensure your deployment YAML uses:

```yaml
image: thanushrii/spring_mysql_kubeadm
```

Apply configurations:

```bash
kubectl apply -f kubescripts/
```

---

## 🔍 Verification Commands

```bash
kubectl get nodes
kubectl get pods
kubectl get deployments
kubectl get services
```

---

## 🌐 Access Application

```bash
kubectl get svc
```

Then open:

```
http://<NODE-IP>:<PORT>
```

---

## ⚠️ Troubleshooting

### Pod Issues

```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

### ImagePullBackOff

* Check Docker image name
* Ensure image is pushed
* Verify Docker Hub login

### Worker Node Join Failed

```bash
kubeadm token create --print-join-command
```

---

## 📚 Tech Stack

* Java (Spring Boot)
* MySQL
* Docker
* Kubernetes (kubeadm)

---

## 👨‍💻 Author

**Thanu**
Aspiring Java Full Stack Developer & DevOps Engineer

---
<img width="1920" height="813" alt="Screenshot (117)" src="https://github.com/user-attachments/assets/66824c35-e7b6-4be1-849e-64a1e321c538" />
<img width="1920" height="951" alt="Screenshot (116)" src="https://github.com/user-attachments/assets/c15298ee-65bd-47e3-9537-c2d37af9e09e" />
<img width="1920" height="970" alt="Screenshot (118)" src="https://github.com/user-attachments/assets/c6599ad6-8b8e-472e-89d5-a37315448a28" />






# 🚀 CI/CD Pipeline: EC2 + Jenkins + Ansible + Docker

---

## 🧠 Project Overview

This project demonstrates a complete **CI/CD pipeline** using:

* **Jenkins** – Continuous Integration & Automation
* **Ansible** – Configuration Management & Deployment
* **Docker** – Containerization
* **AWS EC2** – Hosting Infrastructure

👉 The pipeline automates application deployment from **GitHub → Jenkins → Ansible → Docker → EC2**

---

## 🧱 Infrastructure Overview

| Component      | Details                         |
| -------------- | ------------------------------- |
| Cloud Provider | AWS                             |
| Instance Type  | t3.micro                        |
| OS             | Ubuntu 22.04 LTS / Amazon Linux |
| Networking     | Default VPC                     |
| Access         | SSH using PEM key               |
| Public Access  | EC2 Public IP / DNS             |

---

## 🔧 Software Installation

---

### 1️⃣ Update System

```bash
sudo yum update -y
```

---

### 2️⃣ Install Docker

```bash
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
```

---

### 3️⃣ Install Ansible

```bash
sudo yum install ansible -y
```

---

### 4️⃣ Install Jenkins

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

sudo yum upgrade -y
sudo yum install java-21-amazon-corretto -y
sudo yum install jenkins -y

sudo systemctl enable jenkins
sudo systemctl start jenkins
```

---

## 🔐 Security Group Configuration

Ensure the following ports are open:

* **22** → SSH
* **8080** → Jenkins UI
* **80 / 443** → Application Access

---

## 🧪 Jenkins Job Configuration

### 🔹 Job Name

`Build-and-Deploy-App`

---

### 🔹 Stage 1: Clone Repository

* Repository URL:
  `https://github.com/your-org/your-repo.git`
* Branch: `main`
* Credentials: SSH key / GitHub Token

---

### 🔹 Stage 2: Run Ansible Playbook

Add build step → **Execute Shell**

```bash
ansible-playbook deploy.yml
```

---

## 📜 Ansible Playbook (`deploy.yml`)

```yaml
---
- name: Deploy Docker Container
  hosts: localhost
  become: yes

  tasks:
    - name: Build Docker image
      community.docker.docker_image:
        name: my_app_image
        build:
          path: .
        state: present
        source: build

    - name: Run Docker container
      community.docker.docker_container:
        name: my_app_container
        image: my_app_image:latest
        state: started
        ports:
          - "80:80"
```

---

## 🐳 Dockerfile Example

```Dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y nginx
COPY index.html /var/www/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## ✅ Validation Steps

### 1️⃣ Check Jenkins Build

* Go to Jenkins Dashboard
* Open job → **Console Output**
* Ensure build is successful

---

### 2️⃣ Verify Docker Container

```bash
docker ps
```

---

### 3️⃣ Test Application

```bash
curl http://localhost:80
```

OR open in browser:

```
http://<your-ec2-ip>:80
```

---

## 🧠 Best Practices

* Use **environment variables** for secrets
* Avoid hardcoding credentials
* Add Jenkins user to Docker group:

```bash
sudo usermod -aG docker jenkins
```

* Monitor EC2 usage (`t3.micro` is for testing only)

---

## 🚀 CI/CD Workflow

```text
Developer → GitHub → Jenkins → Ansible → Docker → EC2 → Live App
```

---

## 🎯 Key Takeaways

* Automated deployment pipeline
* Infrastructure + configuration automation
* Containerized application delivery
* Real-world DevOps workflow

---

## 👨‍💻 Author

**Shashank**
DevOps Engineer | Cloud Enthusiast


Just tell me 🚀

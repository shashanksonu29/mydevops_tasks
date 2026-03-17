

# 🚀 Todo CI/CD End-to-End DevOps Project

A **Full-Stack To-Do List Application** built to demonstrate a **complete DevOps CI/CD pipeline** with **Docker, Jenkins, Kubernetes (EKS), Helm, and AWS CloudWatch monitoring**.

This project showcases how a modern DevOps workflow automates building, testing, containerization, deployment, and monitoring of an application.

---

# 📌 Project Architecture

This application consists of the following components:

### Frontend

* **HTML / CSS / JavaScript**
* `index.html` is the main entry point
* Served from a public directory

### Backend

* **Node.js**
* **Express.js Framework**

### Database

* **SQLite**
* Local database file: `todo.db`

---

# 🎯 Project Goals

This project demonstrates how to:

* Containerize an application using **Docker**
* Build a **CI/CD pipeline using Jenkins**
* Push images to **Docker Hub**
* Deploy applications on **AWS EKS**
* Package Kubernetes resources using **Helm**
* Implement **RBAC and Service Accounts**
* Automate the **end-to-end DevOps workflow**
* Configure **Monitoring and Observability using CloudWatch**
* Implement **Alerting using SNS**

---

# ⚙️ DevOps Workflow Demonstrated

This project includes the following DevOps practices:

* ✅ CI/CD Automation
* ✅ Docker Containerization
* ✅ Kubernetes Orchestration
* ✅ Helm Packaging
* ✅ RBAC Security
* ✅ AWS Cloud Deployment
* ✅ Monitoring & Alerts

---

# 🧰 Prerequisites

Install the following tools:

```
Docker
kubectl
eksctl
Helm
AWS CLI
Jenkins
```

AWS CLI must be configured with IAM credentials.

```
aws configure
```

---

# 🔌 Jenkins Required Plugins

Install the following plugins in Jenkins:

* Kubernetes CLI plugin
* Kubernetes
* Kubernetes Client API
* Kubernetes Credentials
* k8s Credential Provider
* Docker
* Docker Pipeline
* Email Extension Template
* Pipeline Stage View
* SonarQube Scanner

---

# 🖥️ Run Application Locally

Create an **Amazon Linux EC2 instance**.

### Install Git

```
yum install git -y
```

Clone the repository

```
cd /opt
git clone https://github.com/vineethsankre/Todo-Application.git
```

Navigate to project

```
cd todo_cicd_end-end_project/backend
```

Install NodeJS

```
curl -fsSL https://rpm.nodesource.com/setup_18.x | bash -
yum install -y nodejs
```

Install dependencies

```
npm install
```

Start the server

```
node server.js &
```

Access the application

```
http://<PublicIP>:3000
```

---

# 🐳 Docker Installation

```
yum install docker -y
systemctl start docker
systemctl enable docker
sudo usermod -aG docker jenkins
sudo chmod 666 /var/run/docker.sock
sudo systemctl restart docker
```

Check version

```
docker -v
```

---

# 🐳 Build Docker Image

```
docker build -t todoimage:v1 .
```

List images

```
docker images
```

Run container

```
docker run -itd -p 3001:3000 todoimage:v1
```

Check containers

```
docker ps
```

Test application

```
curl http://localhost:3001
```

---

# 🧩 Jenkins Installation (Amazon Linux)

```
sudo yum update -y
```

Add Jenkins repo

```
sudo wget -O /etc/yum.repos.d/jenkins.repo \
https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

Import key

```
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

Install Java

```
sudo yum install java-17-amazon-corretto -y
```

Install Jenkins

```
sudo yum install jenkins -y
```

Start Jenkins

```
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

---

# 🔍 SonarQube Setup

Run SonarQube container

```
docker run -itd --name sonar -p 9000:9000 sonarqube
```

Configure in Jenkins

```
Manage Jenkins → Configure System
```

Add:

```
SonarQube Server
SonarQube Token
SonarQube Scanner
```

---

# 📄 sonar-project.properties

Create this file in the **root of the GitHub repository**

```
sonar.projectKey=todo-node-app
sonar.projectName=Todo Node.js App
sonar.projectVersion=1.0
sonar.sources=.
sonar.language=js
sonar.sourceEncoding=UTF-8
```

---

# 🚦 Configure SonarQube Quality Gate

Login to SonarQube

```
Quality Gates → Create
```

Set it as default.

Add Jenkins webhook

```
http://<JENKINS_URL>/sonarqube-webhook/
```

---

# ☸️ Install kubectl

```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin
```

Check version

```
kubectl version --short --client
```

---

# ☸️ Install eksctl

```
curl --silent --location \
https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz \
| tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin
```

Create cluster

```
eksctl create cluster --name my-cluster
```

---

# 🏗️ EKS Cluster using Terraform

Repository

```
https://github.com/adarsh0331/Eks_Cluster_with_terraform.git
```

Setup includes

* S3 bucket for state
* DynamoDB state locking
* EKS cluster provisioning

Update kubeconfig

```
aws eks update-kubeconfig \
--region us-east-1 \
--name my-eks-cluster
```

---

# 📦 Helm Installation

```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Verify

```
helm version
```

---

# 📧 Configure Email in Jenkins

Install plugin

```
Email Extension Plugin
```

Generate **Gmail App Password**

```
Google Account → Security → App Passwords
```

Configure in Jenkins

```
Manage Jenkins → Configure System
```

SMTP settings

```
SMTP Server: smtp.gmail.com
Port: 465
SSL: Enabled
TLS: Enabled
```

---

# 📊 Monitoring with AWS CloudWatch

Create namespace

```
kubectl create namespace amazon-cloudwatch
```

Associate IAM OIDC provider

```
eksctl utils associate-iam-oidc-provider \
--region us-east-1 \
--cluster my-cluster \
--approve
```

Create IAM service account

```
eksctl create iamserviceaccount \
--name cloudwatch-agent \
--namespace amazon-cloudwatch \
--cluster my-cluster \
--region us-east-1 \
--attach-policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy \
--approve
```

---

# 📦 Install CloudWatch Metrics via Helm

Add repo

```
helm repo add eks https://aws.github.io/eks-charts
helm repo update
```

Install

```
helm upgrade --install aws-cloudwatch-metrics eks/aws-cloudwatch-metrics \
--namespace amazon-cloudwatch \
--set clusterName=my-cluster \
--set region=us-east-1 \
--set serviceAccount.create=false \
--set serviceAccount.name=cloudwatch-agent
```

---

# 🔔 Alerts using SNS

Create SNS topic

```
aws sns create-topic --name eks-monitoring-alerts
```

Subscribe email

```
aws sns subscribe \
--topic-arn <SNS_TOPIC_ARN> \
--protocol email \
--notification-endpoint your-email@example.com
```

---

# 🚨 CloudWatch Alarm Example

Create CPU alarm

```
aws cloudwatch put-metric-alarm \
--alarm-name "High-CPU-EKS" \
--metric-name node_cpu_utilization \
--namespace ContainerInsights \
--statistic Average \
--period 300 \
--threshold 80 \
--comparison-operator GreaterThanThreshold \
--evaluation-periods 1
```

---

# 🧪 CPU Stress Test

Create test pod

```
apiVersion: v1
kind: Pod
metadata:
  name: cpu-stress
spec:
  containers:
  - name: stress
    image: progrium/stress
    args: ["--cpu", "4", "--timeout", "600"]
```

Apply

```
kubectl apply -f stress-cpu.yaml
```

---

# 🧹 Cleanup

```
kubectl delete pod cpu-stress
aws cloudwatch delete-alarms --alarm-names "EKS-HighCPU-Alert"
```

---

# 📊 End-to-End Pipeline Flow

```
Developer Push Code → GitHub

        ↓

Jenkins Pipeline Triggered

        ↓

Code Build (Node.js)

        ↓

SonarQube Code Scan

        ↓

Docker Image Build

        ↓

Push Image → DockerHub

        ↓

Deploy to AWS EKS via Helm

        ↓

Monitoring via CloudWatch

        ↓

Alerts via SNS
```

---

# 📌 Technologies Used

| Category                | Tools            |
| ----------------------- | ---------------- |
| Cloud                   | AWS              |
| Containerization        | Docker           |
| CI/CD                   | Jenkins          |
| Container Orchestration | Kubernetes (EKS) |
| Packaging               | Helm             |
| Code Quality            | SonarQube        |
| Monitoring              | CloudWatch       |
| Alerts                  | SNS              |
| Backend                 | Node.js          |
| Database                | SQLite           |

---

# 👨‍💻 Author

**Shashank**

DevOps Engineer | AWS | Kubernetes | CI/CD | Terraform




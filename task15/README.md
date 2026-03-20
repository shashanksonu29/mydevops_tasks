
# 🚀 To-Do CI/CD End-to-End Project

A full-stack **To-Do List application** built to demonstrate a complete **DevOps CI/CD pipeline** with containerization and Kubernetes deployment.

---

## 📌 Project Overview

This project showcases how to build, containerize, and deploy a full-stack application using modern DevOps practices.

### 🔧 Tech Stack

* **Frontend**: HTML, CSS, JavaScript
* **Backend**: Node.js (Express.js)
* **Database**: SQLite
* **Containerization**: Docker
* **Orchestration**: Kubernetes
* **CI/CD**: GitHub Actions / Jenkins (depending on your setup)

---

## 📁 Project Structure

```
todo_cicd_end-end_project/
│
├── public/               # Frontend files (index.html, CSS, JS)
├── server/               # Backend (Node.js + Express)
├── database/             # SQLite DB file
├── Dockerfile            # Docker configuration
├── docker-compose.yml    # Multi-container setup (optional)
├── k8s/                  # Kubernetes manifests
├── .github/workflows/    # CI/CD pipeline (GitHub Actions)
└── README.md             # Project documentation
```

---

## ⚙️ Features

* Add, update, delete tasks
* Persistent storage using SQLite
* REST API integration
* Dockerized application
* CI/CD pipeline automation
* Kubernetes deployment

---

## 🐳 Docker Setup

### Build Docker Image

```bash
docker build -t todo-app .
```

### Run Container

```bash
docker run -d -p 3000:3000 todo-app
```

---

## ☸️ Kubernetes Deployment

### Apply Kubernetes Manifests

```bash
kubectl apply -f k8s/
```

### Check Pods

```bash
kubectl get pods
```

---

## 🔄 CI/CD Pipeline

The pipeline automates:

1. Code push to repository
2. Build Docker image
3. Run tests (if configured)
4. Push image to Docker Hub / Registry
5. Deploy to Kubernetes

---

## 📦 API Endpoints

| Method | Endpoint   | Description   |
| ------ | ---------- | ------------- |
| GET    | /tasks     | Get all tasks |
| POST   | /tasks     | Add new task  |
| PUT    | /tasks/:id | Update task   |
| DELETE | /tasks/:id | Delete task   |

---

## 🚀 How to Run Locally

### Install Dependencies

```bash
npm install
```

### Start Server

```bash
node server.js
```

### Open in Browser

```
http://localhost:3000
```

---

## 📈 Future Enhancements

* Add authentication (JWT)
* Use cloud database (RDS / MongoDB Atlas)
* Implement monitoring (Prometheus + Grafana)
* Add Helm charts for Kubernetes

---

## 👨‍💻 Author

**Shashank**
DevOps Engineer | Cloud Enthusiast

---



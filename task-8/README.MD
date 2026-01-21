21/01/2026 
DevOps Task-8 — Docker Containerization (Amazon Linux)
🎯 Objective

The objective of this task is to learn Docker fundamentals by:

Installing Docker on Amazon Linux

Creating a simple application

Writing a Dockerfile

Building and running Docker images and containers

Practicing essential Docker commands used in DevOps workflows

🧩 Part 1: Environment Setup (Amazon Linux)
🔹 Step 1: Update the System
sudo yum update -y

🔹 Step 2: Install Docker
sudo yum install docker -y

🔹 Step 3: Start and Enable Docker Service
sudo systemctl start docker
sudo systemctl enable docker

🔹 Step 4: Verify Docker Installation
docker --version


Expected output:

Docker version XX.XX.X

🔹 Step 5: (Optional but Recommended) Run Docker Without sudo
sudo usermod -aG docker ec2-user
newgrp docker


Test:

docker ps

🧩 Part 2: Create a Simple Application
🔹 Application Type

Static HTML website using Nginx

The application displays:

Hello from Docker – DevOps Task

🔹 Step 1: Create Project Directory
mkdir devops-docker-task
cd devops-docker-task

🔹 Step 2: Create index.html
vi index.html


Add the following content:

<!DOCTYPE html>
<html>
<head>
  <title>Docker Task</title>
</head>
<body>
  <h1>Hello from Docker – DevOps Task</h1>
</body>
</html>


Save and exit (ESC :wq).

🧩 Part 3: Dockerfile Creation
🔹 Step 1: Create Dockerfile
vi Dockerfile


Add the following:

# Base image
FROM nginx:latest

# Set working directory
WORKDIR /usr/share/nginx/html

# Copy application files
COPY index.html .

# Expose container port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]

🔹 Dockerfile Explanation
Instruction	Description
FROM	Specifies base image
WORKDIR	Sets working directory
COPY	Copies files to container
EXPOSE	Documents container port
CMD	Starts application
🧩 Part 4: Build Docker Image
🔹 Step 1: Build Image
docker build -t devops-docker-task .

🔹 Step 2: Verify Image
docker images


You should see:

devops-docker-task

🧩 Part 5: Run Docker Container
🔹 Step 1: Run Container
docker run -d -p 8080:80 --name docker-task devops-docker-task

🔹 Step 2: Verify Running Container
docker ps

🌐 Access the Application

If running on EC2, open in browser:

http://<EC2-PUBLIC-IP>:8080

⚠️ Important: EC2 Security Group

Ensure the EC2 Security Group allows:

Type	Port	Source
Custom TCP	8080	0.0.0.0/0
✅ Expected Output
Hello from Docker – DevOps Task

🧩 Part 6: Docker Commands Practice
🔹 List Docker Images
docker images

🔹 List Running Containers
docker ps

🔹 List All Containers
docker ps -a

🔹 Stop Container
docker stop docker-task

🔹 Start Container
docker start docker-task

🔹 Remove Container
docker rm docker-task

🔹 Remove Image
docker rmi devops-docker-task

🧠 Key Learnings

Docker installation on Amazon Linux

Difference between image and container

Writing a Dockerfile

Port mapping in Docker

Running containers on EC2

🏗️ Final Architecture
Browser
   ↓
EC2 Instance (Port 8080)
   ↓
Docker Container
   ↓
Nginx
   ↓
HTML Application

✅ Task-8 Status: Completed Successfully 🎯

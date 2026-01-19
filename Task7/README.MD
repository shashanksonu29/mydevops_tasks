19/1/2026
# 🖥️ Linux System Monitoring Automation (TASK-7)

## 📌 Overview
This project implements a **Bash-based system monitoring script** designed for Linux servers (AWS EC2, on-prem, or VM environments).  
The script automates **system health checks, resource monitoring, process tracking, and log management**, similar to real-world DevOps and Cloud Engineer workflows.

---

## 🎯 Objectives
- Monitor critical system information
- Track CPU, memory, and disk usage
- Identify high CPU–consuming processes
- Maintain structured logs with automatic rotation
- Provide a lightweight, cloud-friendly monitoring solution

---

## 🛠️ Technologies Used
- **Linux (Amazon Linux / RHEL-based systems)**
- **Bash Shell Scripting**
- Core Linux utilities:
  - `mpstat`
  - `ps`
  - `df`
  - `free`
  - `awk`
  - `top`

---

## 📂 Project Structure
.
├── system_monitor.sh
├── high_cpu_process.log
├── logs/
│ └── system_report_<date>.log
└── README.md

yaml
Copy code

---

## ⚙️ Features

### ✅ 1. System Information
Displays:
- Hostname
- Current date and time
- System uptime
- Logged-in users

---

### ✅ 2. Resource Monitoring
Tracks:
- **CPU usage** (calculated using `mpstat`)
- **Memory usage** (used vs total)
- **Disk usage** with alerts for partitions exceeding **80%**

---

### ✅ 3. Process Management
- Lists **top 5 CPU-consuming processes**
- Detects processes consuming **more than 70% CPU**
- Logs high CPU processes to `high_cpu_process.log`

---

### ✅ 4. Log Management
- Automatically creates a `logs/` directory
- Saves complete system reports with timestamps
- Rotates (deletes) logs older than **7 days**

---

## 🚀 How to Run

### 1️⃣ Install Required Package
For Amazon Linux / RHEL-based systems:
```bash
sudo yum install sysstat -y
For Amazon Linux 2023:

bash
Copy code
sudo dnf install sysstat -y
Enable and start sysstat:

bash
Copy code
sudo systemctl enable sysstat
sudo systemctl start sysstat
2️⃣ Make Script Executable
bash
Copy code
chmod +x system_monitor.sh
3️⃣ Execute the Script
bash
Copy code
./system_monitor.sh
📄 Sample Output
yaml
Copy code
CPU Usage         : 0.25%
Memory Usage      : 173MB / 916MB (18%)
Disk Usage        : /dev/nvme0n1p1 at 20%
No partitions exceeding 80%
📊 Logs
System reports stored in:

bash
Copy code
logs/system_report_<date>.log
High CPU processes stored in:

lua
Copy code
high_cpu_process.log
🧠 Use Cases
Linux server health monitoring

AWS EC2 instance monitoring

DevOps automation practice

Interview-ready Bash scripting project

📈 Future Enhancements
Schedule execution using cron

Add email or Slack alerts

Integrate with AWS CloudWatch

Convert to containerized monitoring agent

👨‍💻 Author
Shashank Ambati
DevOps Engineer
Linux | AWS | Automation | CI/CD

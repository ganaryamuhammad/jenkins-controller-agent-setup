# Jenkins Distributed CI/CD Lab on Windows + WSL + Docker

Membangun lab Jenkins pribadi dengan arsitektur **Jenkins Controller di Docker** dan **Jenkins Agent di WSL Ubuntu** menggunakan koneksi SSH.

Project ini dibuat untuk belajar konsep **CI/CD, Jenkins Distributed Build, Agent Management, dan Pipeline Automation**.

---

## 📌 Topologi

```text
Windows 11 Laptop
│
├── Docker Desktop
│   └── Jenkins Controller (Container)
│
└── WSL Ubuntu
    └── Jenkins Agent (SSH Node)
```

---

## 🎯 Objective

* Install Jenkins menggunakan Docker
* Setup plugin Jenkins
* Membuat Jenkins Agent di laptop sendiri
* Koneksi Jenkins Controller ke Agent via SSH
* Menjalankan pipeline di remote node

---

## 🖥️ Environment

| Component        | Detail         |
| ---------------- | -------------- |
| OS Host          | Windows 11     |
| Linux            | WSL Ubuntu     |
| Jenkins          | 2.555.1        |
| Java Agent       | OpenJDK 21     |
| Container Engine | Docker Desktop |
| Agent Method     | SSH            |

---

## 🚀 STEP 1 - Install Jenkins via Docker

Pull image Jenkins:

```bash
docker pull jenkins/jenkins:lts
```

Run container:

```bash
docker run -d \
  --name jenkins \
  -p 8089:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```

Check container:

```bash
docker ps
```

---

## 🔐 STEP 2 - Unlock Jenkins

Get initial password:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Open browser:

```text
http://localhost:8089
```

Paste password.

---

## 🔌 STEP 3 - Install Suggested Plugins

Saat first setup pilih:

```text
Install Suggested Plugins
```

Plugin penting:

* Pipeline
* Git
* SSH Build Agents
* Credentials Binding
* Workspace Cleanup
* Matrix Authorization Strategy

---

## 👤 STEP 4 - Create Admin User

Contoh:

```text
Username : admin
Password : ********
```

---

## 🐧 STEP 5 - Prepare Jenkins Agent (WSL Ubuntu)

Update package:

```bash
sudo apt update
```

Install Java 21:

```bash
sudo apt install openjdk-21-jdk -y
```

Check:

```bash
java --version
```

Install SSH Server:

```bash
sudo apt install openssh-server -y
sudo service ssh start
```

Check SSH:

```bash
sudo service ssh status
```

---

## 👥 STEP 6 - Create Linux User for Agent

```bash
sudo adduser jenkinsagent
```

Set password saat proses pembuatan user.

---

## 🌐 STEP 7 - Get WSL IP Address

```bash
ip a
```

Contoh:

```text
192.168.137.28
```

---

## ⚙️ STEP 8 - Create Jenkins Node

Menu:

```text
Manage Jenkins → Nodes → New Node
```

Isi konfigurasi:

| Field           | Value                             |
| --------------- | --------------------------------- |
| Node Name       | jenkins-agent                     |
| Type            | Permanent Agent                   |
| Executors       | 2                                 |
| Remote Root Dir | /home/jenkinsagent/agent          |
| Labels          | linux wsl docker                  |
| Usage           | Use this node as much as possible |
| Launch Method   | Launch agents via SSH             |

---

## 🔑 STEP 9 - Add Credential

Type:

```text
Username with password
```

Isi:

| Field    | Value               |
| -------- | ------------------- |
| Username | jenkinsagent        |
| Password | password user linux |

---

## 🌍 STEP 10 - SSH Host Config

Gunakan IP WSL:

```text
192.168.137.28
```

Host Key Strategy:

```text
Non verifying Verification Strategy
```

---

## ✅ STEP 11 - Launch Agent

Jika sukses:

```text
Connected
Architecture: Linux (amd64)
```

---

## 🧪 STEP 12 - Test Pipeline

Create New Item → Pipeline

Gunakan script:

```groovy
pipeline {
    agent { label 'linux' }

    stages {
        stage('Info') {
            steps {
                sh 'whoami'
                sh 'hostname'
                sh 'pwd'
                sh 'java -version'
            }
        }
    }
}
```

---

## 📷 Result

Console output:

```text
Running on jenkins-agent
whoami -> jenkinsagent
hostname -> DESKTOP-835V43V
Finished: SUCCESS
```

---

## 🧠 Troubleshooting During Setup

### 1. Wrong Password

```text
Failed to authenticate as jenkinsagent
```

Fix:

```bash
sudo passwd jenkinsagent
```

### 2. host.docker.internal Error

```text
Name or service not known
```

Fix: gunakan IP WSL langsung.

### 3. Java Version Error

```text
compiled by more recent version
```

Fix:

```bash
sudo apt install openjdk-21-jdk -y
```

---

## 📚 What I Learned

* Jenkins Controller vs Agent Architecture
* SSH Node Connection
* Dockerized Jenkins Setup
* Linux Agent Administration
* Distributed Build System
* Jenkins Pipeline as Code
* Troubleshooting Network / Auth / Java Issues

---

## 📌 Future Improvement

* GitHub Webhook Integration
* Docker Build Pipeline
* SonarQube Integration
* Nexus Repository
* Kubernetes Deployment
* Multi Agent Jenkins Farm

---

## 👨‍💻 Author

**Muhammad Ganarya Nirwana**
Fresh Graduate Informatics Engineering
DevOps / System Engineer Enthusiast

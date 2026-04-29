# Jenkins Agent Setup on WSL Ubuntu

## 1. Update Package

sudo apt update

## 2. Install Java 21

sudo apt install openjdk-21-jdk -y

## 3. Verify Java

java -version

## 4. Install SSH Server

sudo apt install openssh-server -y

## 5. Start SSH Service

sudo service ssh start

## 6. Create Agent User

sudo adduser jenkinsagent

## 7. Get IP Address

ip a

## 8. Jenkins Node Configuration

- Node Name: jenkins-agent
- Labels: linux wsl docker
- Remote Root Directory: /home/jenkinsagent/agent
- Launch Method: Launch agents via SSH
- Host: Your WSL IP Address

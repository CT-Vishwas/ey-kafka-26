# Lab: Installing Kafka using Docker
In this lab, we will learn how to install and run Apache Kafka using Docker. Docker allows us to quickly set up Kafka without worrying about the underlying system dependencies.

## Prerequisites
- C2 or equivalent cloud virtual machine instance with a running operating system (Ubuntu recommended)
- Docker installed on your system
```bash
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user
docker --version
sudo reboot
```
- Basic knowledge of Docker commands

## Steps to Install Kafka using Docker
[Kafka Docker Installation Guide](https://hub.docker.com/layers/apache/kafka/4.2.2)
1. Pull the Kafka Docker image:
   ```bash
   docker pull apache/kafka:4.3.1
   ```
2. Run a Kafka container:
   ```bash
   docker run -d --name kafka -p 9092:9092 apache/kafka:4.3.1
   ```
3. Verify that Kafka is running:
   ```bash
   docker ps
   ```
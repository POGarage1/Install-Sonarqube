# SonarQube Installation on AWS EC2

This guide provides a **step-by-step setup** for running SonarQube with Docker and Docker Compose on an AWS EC2 instance.  

---

## Overview

This setup uses **Docker Compose** to run:

- **SonarQube**: Code quality and security scanner.  
- **PostgreSQL**: Database for SonarQube data.  

---

## Prerequisites

- AWS EC2 instance (Ubuntu recommended).  
- Default home directory: `/home/ubuntu`.  
- Git installed (`sudo apt-get install git -y`).  

---

## Directory Setup

1. **Clone the repository**:
```bash
git clone <repository-url>
```

2. **After cloning, your folder should contain**:
```bash
docker-compose.yml
install.sh
.env
```

3. **Create a sonarqube directory**
```bash
mkdir -p sonarqube
```

4. **Directory structure now**
```bash
docker-compose.yml
install.sh
.env
sonarqube/
```

5. **Set directory permissions**
```bash
sudo chown -R 1000:1000 ./sonarqube
sudo chmod -R 775 ./sonarqube
```

⚠️ Important: Without setting permissions, docker-compose up -d may fail with:
ERROR Unable to create file /opt/sonarqube/logs/es.log: Permission denied

6. **Install Docker and Docker Compose**

Run the installation script:
```bash
. install.sh
```

This script installs Docker and Docker Compose without requiring sudo or a logout/login afterward.

7. **Start SonarQube and PostgresSQL**

Launch the containers:
```bash
docker compose up -d
```

8. **Check that the containers are running**
```bash
docker ps
```

9. **Verify Database**
    
Ensure the PostgreSQL database and user are set up correctly:
```bash
docker exec -it sonarqube-db psql -U admin -d sonarqube -c "\l"
docker exec -it sonarqube-db psql -U admin -d sonarqube -c "\du"
```
10. **Edit Inbound Rules in Security Groups**
```bash
Type: TCP
Port: 9000
```
⚠️ Important: Without editting Inbound Rules, access SonarQube on Browser may fail with: ERR_CONNECTION_TIMEOUT

12. **Access SonarQube**

After starting, access SonarQube via your browser:
```bash
http://<EC2_PUBLIC_IP>:9000

Default login credentials:

Username: admin

Password: admin
```
Remember to change the default password after first login.

13. **Troubleshoot**
Permission denied when starting containers
```bash
sudo chown -R 1000:1000 ./sonarqube and sudo chmod -R 775 ./sonarqube
```
Container fails to start
```bash
docker-compose logs -f
```
Cannot access SonarQube on browser: ERR_CONNECTION_TIMEOUT
```bash
Ensure port 9000 is open in AWS Security Group
```

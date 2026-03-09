
<img width="1536" height="1024" alt="ChatGPT Image Mar 9, 2026, 02_53_30 PM" src="https://github.com/user-attachments/assets/59d43897-fe1c-411d-a121-24956be83b7d" />

# Zammad Docker Installation Guide
### Deploy Zammad Helpdesk with Docker Compose

## Note

While working on this project, I noticed that the available documentation for deploying Zammad with Docker was either incomplete, outdated, or difficult to follow for beginners.  
To better understand the setup and create a clearer step-by-step process, I decided to document the installation and configuration myself.

This repository therefore serves as a practical and simplified guide for running **Zammad with Docker Compose**, including the commands and explanations needed to reproduce the setup.




# Zammad Helpdesk Deployment with Docker

## Overview

This project demonstrates how to deploy the **Zammad** using **Docker** and **Docker Compose**.

Zammad is an open-source helpdesk and ticketing system used to manage customer communication through multiple channels such as email, chat, and web forms.

Using Docker allows Zammad and its required services to run in **isolated containers**, making deployment simple and reproducible.

---

# Architecture

Zammad requires several backend services to function properly.
When deployed with Docker Compose, the system runs multiple containers:

| Container     | Purpose                                      |
| ------------- | -------------------------------------------- |
| nginx         | Web server that exposes the Zammad interface |
| railsserver   | Zammad backend application                   |
| websocket     | Real-time updates for the web interface      |
| scheduler     | Background jobs                              |
| postgresql    | Database storage                             |
| redis         | Cache and job queue                          |
| memcached     | Memory caching                               |
| elasticsearch | Full text search engine                      |

All containers communicate internally through a **Docker network**.

```
Browser
   │
   ▼
Docker Port 8080
   │
   ▼
Nginx Container
   │
   ▼
Rails Application
   │
 ┌───────────────┬───────────────┬───────────────┐
 ▼               ▼               ▼
PostgreSQL     Redis        Elasticsearch
(Database)     (Cache)         (Search)
```

---

# Requirements

Before starting, install the following software:

* **Docker**
* **Docker Compose**
* **Git**

Verify installation:

```bash
docker --version
docker compose version
git --version
```

---

# Project Setup

## 1 Clone the Zammad Docker Repository

```bash
git clone https://github.com/zammad/zammad-docker-compose.git
cd zammad-docker-compose
```

This repository contains the Docker configuration for running Zammad.

---

## 2 Create Environment Configuration

Copy the default environment file:

```bash
cp .env.dist .env
```

Edit the file:

```bash
nano .env
```

Configure the PostgreSQL credentials:

```
POSTGRES_USER=zammad
POSTGRES_PASS=zammad123
```

Save the file.

---

# Starting the System

Start all containers in the background:

```bash
docker compose up -d
```

Docker will automatically download all required images.

This includes:

* Zammad application image
* PostgreSQL database
* Redis
* Elasticsearch
* Memcached

---

# Checking Running Containers

To verify that the system is running:

```bash
docker ps
```

Example output:

```
CONTAINER ID   IMAGE                        PORTS
dbf86fdae152   ghcr.io/zammad/zammad        0.0.0.0:8080->8080/tcp
40a855e3847e   postgres:17-alpine
f8cabb3d5f4e   redis:8-alpine
068b0081a172   elasticsearch
```

This shows that the containers are active.

---

# Accessing Zammad

Open your browser and navigate to:

```
http://localhost:8080
```

You will see the **Zammad setup wizard**.

Complete the initial configuration:

1. Create the administrator account
2. Configure organization
3. Skip email setup (optional)
4. Skip channels

After this, the helpdesk dashboard will appear.

---

# Useful Docker Commands

### View running containers

```bash
docker ps
```

### View logs

```bash
docker compose logs
```

### Follow logs in real time

```bash
docker compose logs -f
```

### Stop the system

```bash
docker compose down
```

### Restart the system

```bash
docker compose restart
```

### Start containers again

```bash
docker compose up -d
```

---

# Data Persistence

Docker volumes are used to store persistent data.

Important volumes include:

| Volume             | Purpose          |
| ------------------ | ---------------- |
| postgresql-data    | Database data    |
| redis-data         | Redis storage    |
| elasticsearch-data | Search index     |
| zammad-storage     | File attachments |
| zammad-backup      | System backups   |

This ensures that data is not lost when containers restart.

---

# Advantages of Using Docker for Zammad

* Easy deployment
* Consistent environment
* Isolation of services
* Simple scaling
* Easy backup and restore
* Portable setup

Docker allows Zammad to run identically across different systems such as:

* Linux servers
* Windows with Docker Desktop
* Cloud environments
* Development machines

---

# Conclusion

Using Docker Compose simplifies the deployment of Zammad by automatically managing all required services in separate containers.

This setup provides a reliable and scalable environment for running a professional helpdesk system.

---

If you want, I can also show you how to make your README **look much more professional on GitHub** with:

* badges
* architecture diagram
* Docker container diagram
* screenshots
* setup sections like a real open-source project

It will make your repository look **like a professional DevOps project**.


zammad
docker
docker-compose
helpdesk
ticket-system
devops
self-hosted

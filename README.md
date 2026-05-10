# Automated DevOps CI/CD Pipeline with Jenkins & Docker

A complete beginner-to-intermediate DevOps automation project demonstrating how modern engineering teams automate application builds, testing, containerization, and deployment using Jenkins, Docker, GitHub, and CI/CD pipelines.

This project simulates a real-world Continuous Integration and Continuous Deployment (CI/CD) workflow with automated application delivery.

---

# Project Highlights

- Docker containerization
- Jenkins automation server
- CI/CD pipeline implementation
- Automated application deployment
- GitHub integration
- Docker-based Jenkins setup
- Automated version deployment
- Pipeline trigger automation
- DevOps workflow simulation

---

# Architecture Overview

## CI/CD Workflow

```text
Developer Pushes Code
        ↓
GitHub Repository
        ↓
Jenkins Pipeline Trigger
        ↓
Build Docker Image
        ↓
Run Validation Checks
        ↓
Deploy Updated Container
        ↓
Application Available on Localhost
```

---

# Tech Stack

## DevOps Tools

- Jenkins
- Docker
- Git
- GitHub

## Development

- Python
- Flask
- Bash

## Automation

- Jenkins Pipelines
- Docker Networking
- GitHub Webhooks

---

# Project Structure

```bash
.
├── app/
├── Dockerfile
├── Jenkinsfile
├── requirements.txt
├── screenshots/
└── README.md
```

---

# Features

- Automated CI/CD pipeline
- Dockerized web application
- Jenkins running inside Docker
- GitHub repository integration
- Automatic deployment after code push
- Version update automation
- Health check endpoints
- Pipeline monitoring

---

# Learning Objectives

Through this project, I practiced:

- Docker containerization
- Jenkins pipeline configuration
- GitHub integration with Jenkins
- CI/CD automation workflows
- Automated deployments
- Docker networking
- Version management
- Continuous delivery concepts

---

# Prerequisites

Install the following tools:

- Docker
- Git
- Code editor
- GitHub account

---

# Clone Repository

```bash
git clone https://github.com/Chrisblurp/simple-devops-project.git

cd simple-devops-project
```

---

# Run Application Locally

```bash
cd app

python app.py
```

Open in browser:

```text
http://localhost:5000
```

---

# Dockerization

## Build Docker Image

```bash
docker build -t simple-devops-app .
```

---

# Run Docker Container

```bash
docker run -p 5005:5000 simple-devops-app
```

---

# Verify Application

```bash
curl http://localhost:5005
```

---

# Jenkins Setup with Docker

Jenkins was deployed inside a Docker container to simulate a production-style CI/CD automation server.

---

# Create Docker Network

```bash
docker network create devops-net
```

---

# Run Jenkins Container

```bash
docker run -d \
--name jenkins \
--network devops-net \
-p 8080:8080 \
-p 50000:50000 \
-v jenkins_home:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
-v $(which docker):/usr/bin/docker \
--group-add $(stat -c '%g' /var/run/docker.sock) \
jenkins/jenkins:lts-jdk17
```

---

# Retrieve Jenkins Initial Password

```bash
docker exec jenkins \
cat /var/jenkins_home/secrets/initialAdminPassword
```

---

# Jenkins Configuration

## Access Jenkins

Open browser:

```text
http://localhost:8080
```

---

# Jenkins Setup Steps

- Install suggested plugins
- Create admin user
- Configure GitHub credentials
- Create CI/CD pipeline

---

# GitHub Credentials Setup

GitHub Personal Access Token was configured inside Jenkins for repository access and pipeline automation.

---

# CI/CD Pipeline

The Jenkins pipeline automates:

- Source code checkout
- Docker image build
- Application deployment
- Continuous delivery workflow

---

# Jenkins Pipeline Configuration

## Pipeline Source

```text
Pipeline script from SCM
```

## Repository

```text
https://github.com/YOUR_GITHUB_USERNAME/simple-devops-project.git
```

## Branch

```text
main
```

## Script Path

```text
Jenkinsfile
```

---

# Run Pipeline

Inside Jenkins:

```text
Build Now
```

Monitor:

- Build logs
- Console output
- Deployment stages

---

# Auto-Deployment Workflow

The project demonstrates automated deployments after code updates.

---

# Update Application Version

## Modify Application Version

```bash
sed -i '' 's/1.0.0/1.0.1/g' app/app.py
```

---

# Update Dockerfile Version

```bash
sed -i '' 's/APP_VERSION=1.0.0/APP_VERSION=1.0.1/g' Dockerfile
```

---

# Commit & Push Changes

```bash
git add .

git commit -m "Bump version to 1.0.1"

git push origin main
```

---

# Automatic Pipeline Trigger

After pushing code:

- Jenkins automatically detects changes
- Pipeline triggers automatically
- New Docker image is built
- Updated application is deployed

---

# Application Verification

## Check Running Application

```bash
curl http://localhost:5005
```

---

# Health Endpoint

```bash
curl http://localhost:5005/health
```

---

# Version Endpoint

```bash
curl http://localhost:5005/version
```

---

# DevOps Skills Demonstrated

- Docker containerization
- Jenkins administration
- CI/CD automation
- GitHub integration
- Automated deployment workflows
- Docker networking
- Linux command-line operations
- Version management
- Continuous delivery pipelines


# Lessons Learned

Through this project, I gained practical experience with:

- Setting up Jenkins inside Docker
- Creating automated CI/CD pipelines
- Integrating GitHub with Jenkins
- Automating application deployment
- Managing Docker-based workflows
- Implementing continuous deployment practices
- Debugging pipeline failures
- Monitoring automated builds

---


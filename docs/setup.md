# Project Setup Guide

## Prerequisites

The following tools are required:

- Linux
- Git
- Docker
- Jenkins
- GitHub account
- Docker Hub account
- Internet connectivity


## 1.Clone the Repository

```bash
git clone https://github.com/SKrrr0307/final-devops-project.git
cd final-devops-project


## 2.Build Docker Image Locally

```bash
docker build -t final-devops-project .
```


## 3.Run the Application
```bash
docker run -d \
  --name final-devops-local \
  -p 8081:80 \
  final-devops-project
```
The application can be accessed at:
http://localhost:8081

## 4.Verify the Container
```bash
docker ps
```

## 5.Test the Application
```bash
curl http://localhost:8081
```

## Jenkins Setup
The Jenkins pipeline is stored in:
```bash
Jenkinsfile
```
The Jenkins job should be configured to use the GitHub repository.

The Jenkins credential used for Docker Hub authentication is:
```bash
docker-hub-creds
```
The credential should be configured using Jenkins Credentials Manager.


## GitHub Webhook

Configure a GitHub webhook pointing to:

https://<jenkins-webhook-url>/github-webhook/

The webhook allows GitHub pushes to trigger Jenkins automatically.
Pipeline Flow

Git Push
   ↓
GitHub Webhook
   ↓
Jenkins
   ↓
Docker Build
   ↓
Docker Hub Login
   ↓
Docker Push
   ↓
Container Deployment

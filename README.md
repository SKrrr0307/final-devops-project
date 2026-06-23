# Mini DevOps Project

## Overview

This project demonstrates a complete DevOps CI/CD workflow using:

- Git & GitHub
- Docker
- Docker Hub
- Jenkins
- GitHub Webhooks

## Architecture

Developer
↓
GitHub
↓
Webhook
↓
Jenkins Pipeline
↓
Docker Build
↓
Docker Hub Push
↓
Automatic Deployment

## Tools Used

- Git
- GitHub
- Docker
- Docker Hub
- Jenkins
- Linux

## Pipeline Stages

1. Checkout Code
2. Build Docker Image
3. Login to Docker Hub
4. Push Docker Image
5. Deploy Container

## How to Run

```bash
docker build -t final-devops-project .
docker run -d -p 8081:80 final-devops-project
```

## Author

Shubham Kanase

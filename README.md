# DevOps CI/CD Project

A hands-on DevOps project demonstrating an automated CI/CD pipeline using GitHub, Jenkins, Docker, Docker Hub, and GitHub Webhooks.

The project automatically builds a Docker image, pushes the image to Docker Hub with a unique Jenkins build number, and deploys the latest containerized application.

---

##Project Overview

This project demonstrates how source code can move from a developer's GitHub repository to a running Docker container through an automated CI/CD pipeline.

### Workflow

```text
Developer
    |
    v
GitHub Repository
    |
    | GitHub Webhook
    v
Jenkins
    |
    +--> Docker Build
    |
    +--> Docker Hub Login
    |
    +--> Docker Image Push
    |
    +--> Container Deployment
    |
    v
Running Web Application

## Architecture

                 +----------------+
                 |    Developer   |
                 +-------+--------+
                         |
                         | git push
                         v
                 +----------------+
                 |     GitHub     |
                 +-------+--------+
                         |
                         | Webhook
                         v
                 +----------------+
                 |     Jenkins    |
                 +-------+--------+
                         |
              +----------+----------+
              |                     |
              v                     v
       Docker Build           Docker Hub
              |                     |
              +----------+----------+
                         |
                         v
                 Docker Container
                         |
                         v
                 Nginx Web Server
                         |
                         v
                 Web Application

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

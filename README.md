# DevOps CI/CD Project

A hands-on DevOps project demonstrating an automated CI/CD pipeline using GitHub, Jenkins, Docker, Docker Hub, and GitHub Webhooks.

The project automatically builds a Docker image, pushes the image to Docker Hub with a unique Jenkins build number, and deploys the latest containerized application.

---
## Key DevOps Concepts Demonstrated

- Git-based source control
- Webhook-driven CI/CD
- Jenkins pipeline automation
- Docker image creation
- Docker image versioning using Jenkins build numbers
- Docker Hub image publishing
- Automated container deployment
- Jenkins credential management
- Basic deployment troubleshooting
---
## Project Overview

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
```

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

## Technology Stack

| Technology      | Purpose                      |
| --------------- | ---------------------------- |
| Linux           | Development environment      |
| Git             | Version control              |
| GitHub          | Source code repository       |
| GitHub Webhooks | Jenkins pipeline trigger     |
| Jenkins         | CI/CD automation             |
| Docker          | Application containerization |
| Docker Hub      | Container image registry     |
| Nginx           | Web server                   |


## Jenkins Pipeline Stages

The Jenkins pipeline performs these operations

1. Build Docker Image
```bash
docker build -t skrrr0307/final-devops-project:${BUILD_NUMBER} .
```
2. Login to Docker Hub

Jenkins retrieves Docker Hub credentials from Jenkins Credentials Manager.

3. Push Docker Image

```bash
docker push skrrr0307/final-devops-project:${BUILD_NUMBER}
```

4. Deploy Container

The previous application container is stopped and removed before deploying the new version.

```bash
docker stop final-devops || true
docker rm final-devops || true

docker run -d --name final-devops -p 8081:80 skrrr0307/final-devops-project:${BUILD_NUMBER}
```


## Docker Image Versioning

The Project uses Jenkins BUILD_NUMBER to create unique Docker image tags

Example:
```text
Build 1 → final-devops-project:1
Build 2 → final-devops-project:2
Build 3 → final-devops-project:3
```
This provides basic version traceability between Jenkins builds and Docker images.


## Repository Structure
```text
final-devops-project/
│
├── app/
│   └── index.html
│
├── docs/
│   ├── architecture.md
│   ├── setup.md
│   └── troubleshooting.md
│
├── screenshots/
│   └── README.md
│
├── Dockerfile
├── Jenkinsfile
├── README.md
└── .gitignore
```


## Docker Configuration

The application uses Nginx as the base image.
```bash
FROM nginx:latest
COPY app/index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

Build locally:
```bash
docker build -t final-devops-project .
```

Run locally:
```bash
docker run -d \
  --name final-devops-local \
  -p 8081:80 \
  final-devops-project
```

Open:
http://localhost:8081


## Jenkins Configuration

The pipeline is defined in the Jenkinsfile.

Docker Hub authentication uses the Jenkins credential:
docker-hub-creds

Credentials are stored in Jenkins rather than being hardcoded in the repository.


## GitHub Webhook

GitHub sends a webhook request to Jenkins whenever changes are pushed.

The webhook endpoint follows:
https://<jenkins-webhook-url>/github-webhook/

Jenkins then starts the CI/CD pipeline.


## Security

Sensitive files are excluded through .gitignore.

Examples:
```bash
*.pem
*.tfstate
*.tfstate.backup
.terraform/
terraform.tfvars
```
Never commit:

-AWS access keys
-Docker Hub passwords or PATs
-SSH private keys
-Jenkins secrets
-API tokens


## Local Testing

Build the Image:
```bash
docker build -t final-devops-project .
```

Run the container:
```bash
docker run -d -p 8081:80 final-devops-project
```

Check the container:
```bash
docker ps
```
Test the application:
```bash
curl http://localhost:8081
```


## Screenshots

The screenshots/ directory will contain evidence of the CI/CD workflow.

Planned screenshots:

-GitHub repository
-Jenkins pipeline
-Successful Jenkins build
-Docker Hub image
-Running application
-GitHub webhook delivery


## Troubleshooting

Common issues encountered while building this project will be documented under:
```bash
docs/troubleshooting.md
```


## Future Improvements

-Docker Compose
-Automated testing
-Kubernetes deployment
-Terraform infrastructure
-AWS deployment
-Monitoring and logging
-Security scanning
-Rolling deployments

---

## Key Learnings

Through this project, I gained hands-on experience with:

- Designing a basic CI/CD workflow
- Integrating GitHub with Jenkins using webhooks
- Building Docker images through Jenkins
- Using Jenkins Credentials Manager for external authentication
- Publishing versioned Docker images to Docker Hub
- Deploying containers automatically
- Troubleshooting GitHub, Jenkins, Docker, and credential-related issues
- Using Jenkins `BUILD_NUMBER` for Docker image versioning
- Structuring a DevOps project for reproducibility and documentation


## Author

Shubham Kanase

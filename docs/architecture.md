# Project Architecture

## Overview

This project implements a basic automated CI/CD pipeline for deploying a containerized Nginx web application.

The pipeline connects GitHub, Jenkins, Docker, and Docker Hub.

## Architecture Flow

```text
Developer
    |
    | git push
    v
GitHub Repository
    |
    | GitHub Webhook
    v
Jenkins
    |
    +----------------------+
    |                      |
    v                      v
Docker Build         Docker Hub Login
    |                      |
    +----------+-----------+
               |
               v
         Docker Image
               |
               | docker push
               v
          Docker Hub
               |
               v
      Docker Container
               |
               v
          Nginx Server
               |
               v
        Web Application
```


## Components

## GitHub
Stores the application source code, Dockerfile, Jenkinsfile, and project documentation.
 
## GitHub Webhook
Notifies Jenkins when changes are pushed to the repository.

## Jenkins
Automates the CI/CD workflow.

The Jenkins pipeline:

Builds the Docker image.
Authenticates with Docker Hub.
Pushes the image to Docker Hub.
Stops the previous application container.
Deploys the new container.

## Docker
Packages the web application into a portable container.

## Docker Hub
Acts as the container image registry.

## Nginx
Serves the static HTML application inside the Docker container.

## Image Versioning
Docker images are tagged using the Jenkins build number.

Example:

Build #1 → skrrr0307/final-devops-project:1
Build #2 → skrrr0307/final-devops-project:2
Build #3 → skrrr0307/final-devops-project:3

This allows Jenkins builds to be associated with specific Docker image versions.

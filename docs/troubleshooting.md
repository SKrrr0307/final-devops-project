# Troubleshooting Guide

This document contains common issues encountered while building and running the project.

---

## 1. Docker Permission Denied

### Problem

Jenkins may fail when executing Docker commands with a permission error.

Example:

```text
permission denied while trying to connect to the Docker daemon socket
```

### Cause

The Jenkins user does not have permission to communicate with the Docker daemon.

### Resolution

For a lab environment, Jenkins can be given access to the Docker group.

Verify Docker access:
```bash
docker ps
```
The Jenkins environment must be able to execute Docker commands before the pipeline can build or deploy containers.

## 2. Docker Hub Credential Type Error

### Problem

Jenkins may report:

Credentials 'docker-hub-creds' is of type 'Username with password'
where 'org.jenkinsci.plugins.plaincredentials.StringCredentials'
was expected

### Cause

The Jenkinsfile was using a credential binding that expected a different credential type.

### Resolution

Use the Jenkins usernamePassword credential binding:

```bash
withCredentials([
    usernamePassword(
        credentialsId: 'docker-hub-creds',
        usernameVariable: 'DOCKER_USER',
        passwordVariable: 'DOCKER_PASS'
    )
]) {
    sh '''
    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
    '''
}
```
## 3. Docker Image Tagging

### Problem

Using the same tag for every build can make it difficult to identify which build produced an image.

### Resolution

The project uses Jenkins BUILD_NUMBER.

Example:
```bash
final-devops-project:1
final-devops-project:2
final-devops-project:3
```
This provides basic build-to-image traceability.

## 4. Application Port Conflict

### Problem

Port 8080 may already be occupied by Jenkins.

### Resolution

The application is exposed on host port 8081:
```bash
docker run -d -p 8081:80 final-devops-project
```
Jenkins can continue using port 8080.

## 5. Existing Container Name

### Problem

Docker may report that a container named final-devops already exists.

### Resolution

The deployment stage removes the previous container before starting the new one:
```bash
docker stop final-devops || true
docker rm final-devops || true
```
The || true prevents the pipeline from failing if the container does not already exist.

## 6. GitHub Webhook Not Triggering Jenkins

### Possible Causes
Jenkins is not reachable from GitHub.
The webhook URL is incorrect.
The /github-webhook/ endpoint is missing.
The Jenkins job is not configured correctly.
Network tunneling service configuration has changed.

### Verification

Check the webhook delivery status in GitHub.

Verify that the configured endpoint ends with:

/github-webhook/

## 7. Jenkins Data Lost After Restart

### Problem

If Jenkins is started without persistent storage, configuration may be lost when the Jenkins container is removed.

### Cause

The Jenkins home directory:

/var/jenkins_home

was not persisted.

###  Recommended Solution

Use a Docker volume for Jenkins data:
```bash
docker volume create jenkins_home
```
Then mount it to:
```bash
/var/jenkins_home
```
This allows Jenkins configuration and jobs to survive container recreation.

## 8. Security Reminder

Never commit the following to GitHub:
```bash
*.pem
AWS access keys
Docker Hub passwords
Docker Hub PATs
Jenkins secrets
API tokens
terraform.tfvars
Terraform state files
```

Use secret-management mechanisms such as Jenkins Credentials Manager instead.

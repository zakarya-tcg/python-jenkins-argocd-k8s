# End-to-End CI/CD Pipeline for Django Application

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)]()
[![Docker](https://img.shields.io/badge/docker-ready-blue)]()
[![Kubernetes](https://img.shields.io/badge/kubernetes-deployed-blue)]()

This repository contains a simple Django Todo application along with a complete CI/CD pipeline implementation. The project is designed to demonstrate how to automate the build, test, and deployment processes using modern DevOps tools.

## 🏗️ Architecture

The CI/CD pipeline follows a GitOps approach:

**GitHub -> Jenkins -> Docker Hub -> Argo CD -> Kubernetes Cluster**

1. **GitHub**: Source code repository.
2. **Jenkins**: CI server that pulls code, builds the Docker image, and pushes it to Docker Hub. It also updates the Kubernetes manifests in the repository with the new image tag.
3. **Docker Hub**: Container registry storing the built images.
4. **Argo CD**: Continuous Delivery tool running in the Kubernetes cluster that monitors the manifest repository and automatically synchronizes the deployment state.
5. **Kubernetes**: The target environment where the application is deployed and run.

### Pipeline Diagram
![Architecture Diagram](https://user-images.githubusercontent.com/43399466/216001659-74024e94-2c3c-4f1a-8e2e-3ef69b3a88ad.png)

## 🚀 Prerequisites

To run and deploy this project locally, you will need:
- [Docker](https://docs.docker.com/get-docker/) & [Docker Compose](https://docs.docker.com/compose/install/)
- [Kubernetes](https://kubernetes.io/) (e.g., Minikube, Kind, or a cloud provider cluster)
- [Jenkins](https://www.jenkins.io/)
- [Argo CD](https://argo-cd.readthedocs.io/en/stable/)

## 💻 Local Development

You can run the application locally either using Python directly or via Docker Compose.

### Option 1: Using Python
Make sure you have Python 3 installed.
```bash
pip install django==3.2
python manage.py migrate
python manage.py runserver
```

### Option 2: Using Docker Compose
```bash
docker-compose up --build
```
The application will be accessible at `http://localhost:8000`.

## ⚙️ CI/CD Configuration (Jenkinsfile)

The `Jenkinsfile` defines the following stages:
- **Checkout**: Pulls the source code from GitHub.
- **Build Docker**: Builds a new Docker image labeled with the Jenkins build number (`BUILD_NUMBER`).
- **Push the artifacts**: Pushes the newly built image to Docker Hub.
- **Checkout K8S manifest SCM**: Pulls the separate Git repository used for Kubernetes manifests.
- **Update K8S manifest & push to Repo**: Updates the Kubernetes deployment manifest (`deploy.yaml`) with the new image tag and pushes the change back to GitHub, triggering Argo CD to sync the state.

## 🐳 Kubernetes Deployment

The `deploy/` directory contains standard Kubernetes manifests:
- `deploy.yaml`: Deployment configuration for the Django app.
- `service.yaml`: Service to expose the application.
- `pod.yaml`: Sample pod specification.

To apply them manually:
```bash
kubectl apply -f deploy/
```

## 📺 Video Tutorial

You can find the complete details of the setup and configuration in this video tutorial:
[Watch on YouTube](https://www.youtube.com/watch?v=ogrx8G8pClQ)

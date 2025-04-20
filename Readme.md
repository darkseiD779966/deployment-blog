# 🚀 Backend Blog Deployment (NestJS) - CI/CD to Kubernetes

This repository contains the CI/CD setup and Kubernetes configuration to build, push, and deploy a Dockerized NestJS backend application using Azure DevOps Pipelines and Kubernetes.

---

## 📦 Folder Structure

deployment/ ├── cicd-pipelines/ │ └── blog-cicd-pipeline.yaml # Azure DevOps pipeline for CI/CD ├── k8s-specifications/ │ └── backend-blog-deployment.yaml # Kubernetes deployment manifest └── scripts/ └── updateK8sManifests.sh # Script to auto-update image in manifest

yaml
Copy
Edit

---

## 🔧 Technologies Used

- **NestJS** (backend service)
- **Docker** (containerization)
- **Azure DevOps Pipelines** (CI/CD orchestration)
- **Kubernetes** (deployment target)
- **Shell Scripting** (manifest automation)

---

## ⚙️ CI/CD Pipeline Overview

### 🏗️ Stage 1: Build & Push Docker Image

- Triggered on changes to `main` branch and `backend-blog/**` files.
- Builds Docker image using `backend-blog/Dockerfile`.
- Pushes the image to the specified container registry (`lm-docker-con`).
- Captures and sets the build ID as a version tag.

### 🛠️ Stage 2: Update Kubernetes Manifests

- Shell script (`updateK8sManifests.sh`) updates the Kubernetes manifest.
- Replaces the image tag in the deployment YAML with the current `Build ID`.

### 🚀 Stage 3: Deploy to Kubernetes

- Uses Azure DevOps `KubernetesManifest@1` task.
- Applies the updated `backend-blog-deployment.yaml`.
- Deployment target: `default` namespace in the `lm-con` cluster.

---

## 📄 Kubernetes Deployment Spec (Simplified)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blog-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: blog-app
  template:
    metadata:
      labels:
        app: blog-app
    spec:
      containers:
      - name: blog-container
        image: <updated dynamically>
        ports:
        - containerPort: 3000
      imagePullSecrets:
      - name: <your-secret-name>
🐚 Manifest Update Script
scripts/updateK8sManifests.sh automates:

Cloning the repo

Checking out main

Updating image tag with sed

Committing and pushing the change

🧪 Pipeline Trigger
yaml
Copy
Edit
trigger:
  branches:
    include:
      - main
  paths:
    include:
      - backend-blog/**
📌 Notes
Ensure proper Docker registry and Kubernetes service connections are configured in Azure DevOps (lm-docker-con and lm-con).

Kubernetes cluster must allow access to private Docker images via imagePullSecrets.
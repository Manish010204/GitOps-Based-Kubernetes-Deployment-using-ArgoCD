# 🚀 GitOps-Based Kubernetes Deployment using ArgoCD

## 📖 Table of Contents
- [Overview](#overview)
- [Objectives](#objectives)
- [Architecture](#architecture)
- [Why GitOps](#why-gitops)
- [Tech Stack](#tech-stack)
- [Repository Structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Setup & Installation](#setup--installation)
- [CI Pipeline](#ci-pipeline)
- [Kubernetes Deployment](#kubernetes-deployment)
- [ArgoCD Configuration](#argocd-configuration)
- [GitOps Workflow](#gitops-workflow)
- [Rollback Strategy](#rollback-strategy)
- [Key Features](#key-features)
- [Results](#results)
- [Challenges](#challenges)
- [Future Enhancements](#future-enhancements)
- [Author](#author)

---

## 📌 Overview
This project implements a **GitOps-based CI/CD pipeline** for Kubernetes using GitHub Actions and ArgoCD.

It automates application delivery by treating Git as the **single source of truth**, ensuring:
- Continuous Deployment  
- Automatic Synchronization  
- Self-Healing Infrastructure  
- Version-controlled Rollback  

---

## 🎯 Objectives
- Implement GitOps principles for Kubernetes deployments  
- Automate CI/CD using GitHub Actions  
- Ensure continuous synchronization and drift correction  
- Enable seamless rollback using Git versioning  
- Build a scalable deployment pipeline  

---

## 🏗️ Architecture

### 🔄 Workflow
```
Developer pushes code → GitHub Actions builds image → Docker Hub → ArgoCD → Kubernetes
```

---

## 🤔 Why GitOps?

- Git as **single source of truth**
- Easy rollback using commits
- Automatic synchronization
- Reduced manual errors

---

## 🧱 Tech Stack

| Layer | Technology |
|------|-----------|
| Containerization | Docker |
| CI/CD | GitHub Actions |
| GitOps | ArgoCD |
| Orchestration | Kubernetes |
| Backend | Python (Flask) |

---

## 📁 Repository Structure

```
gitops-project/
│
├── app/
│   ├── app.py
│   └── Dockerfile
│
├── k8s-manifests/
│   ├── deployment.yaml
│   └── service.yaml
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
└── README.md
```

---

## ⚙️ Prerequisites

- Docker  
- Kubernetes  
- kubectl  
- Git  
- Docker Hub  
- ArgoCD CLI  

---

## 🛠️ Setup & Installation

### Clone Repository
```
git clone https://github.com/<your-username>/gitops-project.git
cd gitops-project
```

### Run Locally
```
docker build -t gitops-app .
docker run -p 5000:5000 gitops-app
```

---

## 🔄 CI Pipeline

```
name: CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Build Docker Image
        run: docker build -t <your-dockerhub-username>/gitops-app:${{ github.sha }} .

      - name: Login to DockerHub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

      - name: Push Docker Image
        run: docker push <your-dockerhub-username>/gitops-app:${{ github.sha }}

      - name: Update Manifest
        run: |
          sed -i "s|image: .*|image: <your-dockerhub-username>/gitops-app:${{ github.sha }}|g" k8s-manifests/deployment.yaml

      - name: Commit Changes
        run: |
          git config --global user.email "you@example.com"
          git config --global user.name "github-actions"
          git add .
          git commit -m "update image"
          git push
```

---

## 🔐 GitHub Secrets

- DOCKER_USERNAME  
- DOCKER_PASSWORD  

---

## ☸️ Kubernetes Deployment

### deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gitops-app
spec:
  replicas: 2
```

### service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: gitops-service
```

---

## ⚡ ArgoCD Configuration

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

## 🔁 GitOps Workflow

```
Code → CI → Docker → Git Update → ArgoCD → Kubernetes
```

---

## 🔄 Rollback

```
git revert <commit-id>
git push origin main
```

---

## ⭐ Key Features
- Continuous Deployment  
- Self-Healing  
- Git-based Rollback  

---

## ⚠️ Challenges
- Image tagging  
- CI sync  

---

## 🚀 Future Enhancements
- Helm  
- Monitoring  
- Multi-env  

---

## 👨‍💻 Author
Manish Thakur


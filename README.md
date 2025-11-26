# ☸️ EKS Jenkins CI/CD Kubernetes Manifests

<div align="center">

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS_EKS-FF9900?style=for-the-badge&logo=amazoneks&logoColor=white)

**Kubernetes Manifests for Automated CI/CD Pipeline with Jenkins on AWS EKS**

[Features](#-features) • [Architecture](#-architecture) • [Quick Start](#-quick-start) • [Configuration](#-configuration)

</div>

---

## 📖 Overview

This repository contains **declarative Kubernetes manifests** for deploying a complete CI/CD pipeline using Jenkins on AWS EKS. The setup automatically builds, pushes, and deploys containerized applications across isolated namespaces.

### 🎯 What's Included

```
✅ Jenkins Deployment with Persistent Storage
✅ Application Deployment with LoadBalancer
✅ RBAC Configuration for Jenkins
✅ Automated CI/CD Pipeline (Jenkinsfile)
✅ Deployment Scripts for Easy Setup
✅ Separate Namespaces (jenkins & app)
```

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 🔧 Jenkins Setup
- **Persistent Storage** with PVC (10GB)
- **ServiceAccount** with RBAC permissions
- **LoadBalancer** for external access
- **Health Checks** (Liveness & Readiness)
- **Resource Limits** configured

</td>
<td width="50%">

### 📦 Application Deployment
- **Multi-replica** deployment (2 pods)
- **LoadBalancer** service
- **Rolling Updates** strategy
- **Resource management**
- **Health probes** enabled

</td>
</tr>
<tr>
<td width="50%">

### 🔒 Security & Isolation
- **Namespace isolation** (jenkins/app)
- **RBAC** policies
- **ServiceAccount** per namespace
- **Security contexts** configured

</td>
<td width="50%">

### 🚀 Automation
- **GitHub webhook** integration
- **Automated builds** on push
- **Docker image** creation
- **Auto-deployment** to Kubernetes

</td>
</tr>
</table>

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    AWS EKS Cluster                          │
│                                                             │
│  ┌────────────────────────┐    ┌────────────────────────┐   │
│  │   Namespace: jenkins   │    │    Namespace: app       │   │
│  │                        │    │                         │   │
│  │  ┌──────────────────┐  │    │  ┌──────────────────┐   │   │
│  │  │  Jenkins Pod     │  │    │  │  App Pod 1       │   │   │
│  │  │  + PVC (10GB)    │  │    │  │                  │   │   │
│  │  │  + ServiceAccount│  │    │  ├──────────────────┤   │   │
│  │  └──────────────────┘  │    │  │  App Pod 2       │   │   │
│  │          │             │    │  │                  │   │   │
│  │  ┌──────────────────┐  │    │  └──────────────────┘   │   │
│  │  │  LoadBalancer    │  │    │          │              │   │
│  │  │  Port: 8080      │  │    │  ┌──────────────────┐   │   │
│  │  └──────────────────┘  │    │  │  LoadBalancer    │   │   │
│  │          │             │    │  │  Port: 80        │   │   │
│  └──────────┼─────────────┘    │  └─────────────── ──┘   │   │
│             │                  │          │              │   │
└─────────────┼──────────────────┼──────────┼───────── ────────┘
              │                  │          │
              ▼                  │          ▼
      Jenkins UI Access          │   Application Access
       (ELB endpoint)            │    (ELB endpoint)
                                 │
                    ┌────────────▼───────────┐
                    │   CI/CD Pipeline       │
                    │                        │
                    │  1. GitHub Push        │
                    │  2. Webhook Trigger    │
                    │  3. Build Image        │
                    │  4. Push to DockerHub  │
                    │  5. Deploy to K8s      │
                    └────────────────────────┘
```

---

## 📁 Repository Structure

```
EKS-Jenkins-CICD-Kubernetes-Manifests/
│
├── README.md                          # This file
├── .gitignore                         # Git ignore rules
│
├── namespaces/                        # Namespace definitions
    ├── jenkins-namespace.yaml         # Jenkins namespace
    |                 ├── jenkins/                           # Jenkins deployment files
    |                      │   ├── jenkins-serviceaccount.yaml    # ServiceAccount for Jenkins
    |                      │   ├── jenkins-rbac.yaml              # RBAC permissions
    |                      │   ├── jenkins-pvc.yaml               # Persistent Volume Claim
    |                      │   ├── jenkins-deployment.yaml        # Jenkins deployment
    |                      │   └── jenkins-service.yaml           # LoadBalancer service
    |                      │
    |                      ├── scripts/                           # Deployment automation scripts
    |                      │   ├── deploy-jenkins.sh              # Deploy Jenkins
    |                      │   ├── deploy-app.sh                  # Deploy application
    |                      │   └── cleanup.sh                     # Cleanup resources
    |                      │
    |                      └── jenkins-config/                    # Jenkins pipeline configuration
    |                          └── Jenkinsfile                    # CI/CD pipeline definition
    |
    └── app-namespace.yaml             # Application namespace
          ├── application/                       # Application deployment files
                  ├── app-deployment.yaml            # App deployment
                  └── app-service.yaml               # LoadBalancer service


```

---

## 🚀 Quick Start

### Prerequisites

Ensure you have the following:

```bash
✓ AWS EKS Cluster running (from Terraform repo)
✓ kubectl configured to access the cluster
✓ DockerHub account
✓ GitHub repository with your application
```

---

## 📊 Resource Specifications

### Jenkins Pod

| Resource | Value |
|----------|-------|
| **Memory Request** | 1Gi |
| **Memory Limit** | 2Gi |
| **CPU Request** | 500m |
| **CPU Limit** | 1000m |
| **Storage** | 10Gi (PVC) |
| **Replicas** | 1 |

### Application Pods

| Resource | Value |
|----------|-------|
| **Memory Request** | 128Mi |
| **Memory Limit** | 256Mi |
| **CPU Request** | 100m |
| **CPU Limit** | 200m |
| **Replicas** | 2 |

---

## 🔄 CI/CD Pipeline Flow

```
┌──────────────┐
│   Developer  │
│  Push Code   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   GitHub     │
│   Webhook    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Jenkins    │
│   Triggered  │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Build Docker │
│    Image     │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  Push to     │
│  DockerHub   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  Deploy to   │
│  Kubernetes  │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Application  │
│    Live!     │
└──────────────┘
```
---

## 🔗 Related Resources

- 🔗 [Terraform Infrastructure Repository](https://github.com/Mohamed-Sarhan-Elhussieny/AWS-EKS-Core-Infrastructure-with-Terraform-depi) - EKS cluster provisioning
- 📚 [Jenkins Documentation](https://www.jenkins.io/doc/)
- 📚 [Kubernetes Documentation](https://kubernetes.io/docs/)
- 📚 [AWS EKS Best Practices](https://aws.github.io/aws-eks-best-practices/)

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Mohamed Sarhan Elhussieny**

⭐ **Star this repo** if you find it helpful!

---

<div align="center">

**Built with ❤️ for DevOps Automation**

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)

</div>

# ☸️ EKS Jenkins CI/CD Kubernetes Manifests

<div align="center">

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS_EKS-FF9900?style=for-the-badge&logo=amazoneks&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)

**Kubernetes Manifests for Automated CI/CD Pipeline with Jenkins on AWS EKS**

[Features](#-features) • [Architecture](#-architecture) • [Quick Start](#-quick-start) • [Configuration](#-configuration)

</div>

---

## 📖 Overview

This repository contains **declarative Kubernetes manifests** for deploying a complete CI/CD pipeline using Jenkins on AWS EKS. The setup automatically builds, pushes, and deploys containerized applications across isolated namespaces with full monitoring capabilities.

### 🎯 What's Included

```
✅ Jenkins Deployment with Persistent Storage
✅ Application Deployment with LoadBalancer
✅ RBAC Configuration for Jenkins
✅ Automated CI/CD Pipeline (Jenkinsfile)
✅ Deployment Scripts for Easy Setup
✅ Separate Namespaces (jenkins & app)
✅ Prometheus & Grafana Monitoring Stack
✅ Real-time Metrics & Dashboards
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
- **Kaniko** for Docker builds without daemon

</td>
<td width="50%">

### 📦 Application Deployment
- **Multi-replica** deployment (2 pods)
- **LoadBalancer** service
- **Rolling Updates** strategy
- **Resource management**
- **Health probes** enabled
- **Zero Downtime** deployments

</td>
</tr>
<tr>
<td width="50%">

### 🔒 Security & Isolation
- **Namespace isolation** (jenkins/app)
- **RBAC** policies
- **ServiceAccount** per namespace
- **Security contexts** configured
- **IAM integration**
- **Private Subnets** support

</td>
<td width="50%">

### 🚀 Automation
- **GitHub webhook** integration
- **Automated builds** on push
- **Docker image** creation with Kaniko
- **Auto-deployment** to Kubernetes
- **Easy Rollback** with Git commits

</td>
</tr>
<tr>
<td width="50%">

### 📊 Monitoring & Observability
- **Prometheus** metrics collection
- **Grafana** dashboards
- **Real-time visibility**
- **Pod/Node/Service** monitoring
- **CPU & Memory** tracking
- **Cluster Performance** insights

</td>
<td width="50%">

### ⚡ Performance
- **Fast delivery** (under 5 minutes)
- **High Availability**
- **Auto-scaling** capabilities
- **Horizontal Pod Autoscaling**
- **Multi-node Architecture**
- **Auto-healing**

</td>
</tr>
</table>

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          AWS EKS Cluster                                │
│                                                                         │
│  ┌────────────────────────┐    ┌────────────────────────┐              │
│  │   Namespace: jenkins   │    │    Namespace: app       │              │
│  │                        │    │                         │              │
│  │  ┌──────────────────┐  │    │  ┌──────────────────┐   │              │
│  │  │  Jenkins Pod     │  │    │  │  App Pod 1       │   │              │
│  │  │  + PVC (10GB)    │  │    │  │                  │   │              │
│  │  │  + ServiceAccount│  │    │  ├──────────────────┤   │              │
│  │  │  + Kaniko        │  │    │  │  App Pod 2       │   │              │
│  │  └──────────────────┘  │    │  │                  │   │              │
│  │          │             │    │  └──────────────────┘   │              │
│  │  ┌──────────────────┐  │    │          │              │              │
│  │  │  LoadBalancer    │  │    │  ┌──────────────────┐   │              │
│  │  │  Port: 8080      │  │    │  │  LoadBalancer    │   │              │
│  │  └──────────────────┘  │    │  │  Port: 80        │   │              │
│  │          │             │    │  └──────────────────┘   │              │
│  └──────────┼─────────────┘    │          │              │              │
│             │                  │          │              │              │
│  ┌──────────┼──────────────────┼──────────┼──────────────┐              │
│  │          │  Monitoring Stack│          │              │              │
│  │  ┌───────▼────────┐  ┌──────▼─────┐   │              │              │
│  │  │  Prometheus    │  │  Grafana   │   │              │              │
│  │  │  (Metrics)     │  │(Dashboards)│   │              │              │
│  │  └────────────────┘  └────────────┘   │              │              │
│  └─────────────────────────────────────────────────────────────────────┘│
│             │                  │          │                             │
└─────────────┼──────────────────┼──────────┼─────────────────────────────┘
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
                    │  3. Build Image(Kaniko)│
                    │  4. Push to DockerHub  │
                    │  5. Rolling Update     │
                    │  6. Zero Downtime      │
                    └────────────────────────┘
```

---

## 📁 Repository Structure

```
EKS-Jenkins-CICD-Kubernetes-Manifests/
│
├── README.md                          # This file
├── LICENSE                            # MIT License
├── Jenkinsfile                        # CI/CD pipeline definition
├── Dockerfile                         # Application Docker image
│
├── create-jenkins-pod/                # Jenkins deployment files
│   ├── deployment.yaml                # Jenkins deployment
│   ├── service1-for-jenkins.yaml      # LoadBalancer service
│   ├── serviceAccount.yaml            # ServiceAccount for Jenkins
│   ├── volume.yaml                    # Persistent Volume Claim
│   │
│   └── Kaniko/                        # Kaniko configuration
│       ├── kaniko.yml                 # Kaniko pod for Docker builds
│       └── docker-auth/               # Docker credentials
│
├── node-2-deploy.yml                  # Application deployment
├── service2-for-deploy-node.yaml      # Application LoadBalancer
│
└── page.html                          # Application HTML page
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
✓ 7 EKS Add-ons configured
✓ VPC with Public & Private Subnets
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
| **Replicas** | 2 (scalable to 4) |

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
│Image (Kaniko)│
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
│Rolling Update│
│  Kubernetes  │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Zero Downtime│
│ Deployment!  │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Prometheus & │
│   Grafana    │
│  Monitoring  │
└──────────────┘

⏱️ Total Time: Under 5 minutes
```

---

## 🎯 Key Achievements

- ✅ **Fully automated deployment** from code to production
- ✅ **Zero downtime** with Rolling Updates strategy
- ✅ **Fast delivery** under 5 minutes per deployment
- ✅ **High availability** with multi-node architecture
- ✅ **Auto-scaling** from 2 to 4 nodes dynamically
- ✅ **Security** with IAM, RBAC, and Private Subnets
- ✅ **Full observability** with Prometheus & Grafana
- ✅ **Easy rollback** linked to Git commits
- ✅ **Infrastructure as Code** with complete automation

---

## 🔗 Related Resources

- 🔗 [Terraform Infrastructure Repository](https://github.com/Mohamed-Sarhan-Elhussieny/AWS-EKS-Core-Infrastructure-with-Terraform-depi-) - EKS cluster provisioning with 7 Add-ons
- 📚 [Project Documentation](https://drive.google.com/drive/folders/1-hD38hDGvNiduf-d6QLJC9lVuf1pDE4t) - Complete project documentation
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
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)

</div>

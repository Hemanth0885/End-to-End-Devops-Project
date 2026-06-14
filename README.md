# End-to-End DevOps Project

Project Overview

This project demonstrates an end-to-end DevOps implementation covering application containerization, CI/CD automation, Kubernetes deployment, infrastructure configuration, and monitoring.

The objective is to automate the software delivery lifecycle using modern DevOps tools and practices.

---

Tools & Technologies:

- Powershell
- WSL
- Docker
- GitHub Actions
- Kubernetes(K3)
- Helm charts
- ArgoCD
- Prometheus
- Grafana

---

## Architecture

```text
Developer Pushes Code
        ↓
GitHub Repository
        ↓
GitHub Actions CI/CD
        ↓
Docker Image Build
        ↓
DockerHub 
        ↓
Kubernetes Cluster(K3)
        ↓
Helm Charts
        ↓
    ArgoCD
        ↓
Prometheus Monitoring
        ↓
Grafana Dashboards

```


---

## Project Components

Configuration
- Installed and configured Linux, K3 Cluster, Prometheus, Grafana, Kube-metrics in WSL- Powershell.

Containerization
- Build Docker images for application deployment

CI/CD
- Automate build and deployment using GitHub Actions

Kubernetes
- Deploy applications using Kubernetes manifests
- Manage services and deployments

Monitoring
- Monitor cluster and application health using Prometheus and Grafana

---


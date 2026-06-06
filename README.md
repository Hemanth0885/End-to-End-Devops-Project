# End-to-End DevOps Project

Project Overview

This project demonstrates an end-to-end DevOps implementation covering application containerization, CI/CD automation, Kubernetes deployment, infrastructure provisioning, and monitoring.

The objective is to automate the software delivery lifecycle using modern DevOps tools and practices.

---

Tools & Technologies:

- Docker
- GitHub Actions
- Kubernetes(K3)
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
DockerHub / Container Registry
        ↓
Kubernetes Cluster(K3)
        ↓
Prometheus Monitoring
        ↓
Grafana Dashboards
```


---

## Project Components

Infrastructure as Code
- Provision AWS infrastructure using Terraform
- Manage resources using reusable modules

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


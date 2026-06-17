End-to-End DevOps Project

Overview
This project demonstrates a complete DevOps implementation covering containerization, CI/CD automation, Kubernetes deployment, GitOps-based continuous delivery, and observability. The goal was to automate the full software delivery lifecycle — from a code commit to a live, monitored deployment — using industry-standard tools and practices.

Credits & Disclaimer
The base application code used in this project is sourced from Abhishek Veeramalla's repository, used purely for learning and DevOps practice purposes. All infrastructure design, CI/CD pipeline configuration, Helm charts, Kubernetes manifests, GitOps setup, and monitoring stack in this repository are designed and implemented by me.

---

Tools/tech that was used: Env-WSL2, PowerShell, LinuxContainerization DockerCI/CDGitHub Action, Orchestration-Kubernetes (K3s)Package, Management-Helm, GitOps / CDArgoCD, Monitoring-Prometheus, Grafana, kube-state-metrics, Registry-DockerHub, other tools-VScode.

---

Architecture

```text

Developer pushes code to GitHub
        ↓
GitHub Actions CI/CD triggers
        ↓
Build & test Go application
        ↓
Build Docker image → push to DockerHub
        ↓
Pipeline auto-updates image tag in Helm values.yaml
        ↓
ArgoCD detects Git change (GitOps)
        ↓
ArgoCD syncs Helm chart to Kubernetes (K3s)
        ↓
Rolling update of application pods
        ↓
Prometheus scrapes metrics → Grafana dashboards

```


---

Project Components

Infrastructure Setup
Provisioned a local Kubernetes (K3s) cluster on WSL2, configured kube-state-metrics, and installed the Prometheus and Grafana monitoring stack.
Containerization
Wrote a multi-stage Dockerfile to build a lightweight, production-ready image for the Go web application.

CI/CD Pipeline (GitHub Actions)
Built a four-stage pipeline: build & unit test → lint (golangci-lint) → Docker build & push to DockerHub (tagged with the GitHub run ID) → automated commit that updates the image tag in the Helm chart's values.yaml, triggering the GitOps flow.
![Multi-stage Dockerfile build artifact](docs/screenshots/Go-web-app.png)
![GitHub Actions CI/CD pipeline success](docs/screenshots/Pipeline-success.png)

Kubernetes & Helm
Created a parameterized Helm chart covering Deployment, Service, ServiceAccount, Ingress, and HPA templates, with environment-specific values managed through values.yaml.
![K3s Kubernetes cluster running](docs/screenshots/K3-cluster.png)

GitOps with ArgoCD
Configured an ArgoCD Application resource pointing at the Helm chart path in the Git repo, with automated sync, self-heal, and pruning enabled so cluster state always matches Git.
![ArgoCD application synced and healthy](docs/screenshots/ArgoCD.png)

Monitoring
Deployed Prometheus to scrape cluster and application metrics, and Grafana dashboards to visualize pod health, resource usage, and deployment status in real time.
Challenges & Troubleshooting
![Grafana dashboard showing pod metrics](docs/screenshots/Grafana-dashboard.png)


->Real issues encountered and resolved during this project:

Immutable selector conflict in ArgoCD sync — ArgoCD repeatedly failed to sync because the existing Deployment's spec.selector differed from the one rendered by Helm, and Kubernetes treats that field as immutable post-creation. Resolved by deleting the stale Deployment object and letting ArgoCD recreate it cleanly from the Helm chart.

CrashLoopBackOff from port mismatch — The application's container port, liveness probe, and readiness probe were misconfigured against the wrong port, causing connection-refused errors and continuous restarts. Fixed by aligning containerPort, livenessProbe, and readinessProbe with the actual port the Go application listens on (8080).

Helm chart directory and template errors — Encountered Chart.yaml file is missing and apiVersion not set, kind not set errors caused by running Helm commands from the wrong directory and an incomplete Deployment template. Fixed by correcting the working directory and rebuilding the Deployment template with the proper apiVersion, kind, and spec structure.

ArgoCD stuck OutOfSync despite automated sync policy — Even with automated.enabled: true, ArgoCD did not immediately reconcile after a Git change. Diagnosed using kubectl describe application to inspect sync conditions and operation state, then resolved with a manual hard refresh and forced sync via kubectl patch.
End-to-end pipeline verification — Validated the complete GitOps loop by making a live code change, confirming it flowed through GitHub Actions, DockerHub, Helm's values.yaml, and ArgoCD, and finally appeared on the running application — confirming the automation works as designed, not just in theory.

---


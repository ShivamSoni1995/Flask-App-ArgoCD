# 🚀 GitOps-Based Flask App Deployment with Jenkins, Docker, and ArgoCD

This project demonstrates a complete CI/CD and GitOps pipeline where:

- A **Flask app** is containerized with Docker
- **Jenkins** builds and pushes a uniquely tagged image to Docker Hub
- The **Kubernetes `deployment.yml`** is auto-updated with the new image tag
- **ArgoCD** watches the Git repo and automatically deploys updates to a Kubernetes cluster

---

## 📦 Tech Stack

- 🐳 Docker
- 🛠️ Jenkins (Pipeline + Webhook-based)
- ☸️ Kubernetes
- 🔄 ArgoCD
- 📁 GitHub
- 🔐 Docker Hub

---


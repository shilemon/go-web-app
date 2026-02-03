# 🚀 Go Web App – End-to-End DevOps CI/CD with GitOps on AWS EKS

This project demonstrates a **production-grade DevOps pipeline** for a **Go web application**, implementing **CI/CD, GitOps, Kubernetes, Helm, and AWS EKS** best practices.

The goal of this project is to show **how modern cloud-native applications are built, packaged, deployed, and exposed automatically** from code push to production.

---

## 🧠 Project Overview

This is a simple **Go-based HTTP web application** that runs inside a Docker container and is deployed to a **Kubernetes cluster (Amazon EKS)** using **Helm and Argo CD**.

The entire deployment lifecycle is automated using **GitHub Actions (CI)** and **Argo CD (CD)** following **GitOps principles**.

---

## 🏗️ Architecture Overview

**High-level flow:**

Developer → GitHub → GitHub Actions (CI)
→ Docker Image Build & Tag
→ Helm Chart Update
→ Argo CD Sync
→ AWS EKS (Deployment, Service, Ingress)
→ Load Balancer → DNS → Users


---

## 🧰 Tech Stack

| Category | Tool |
|--------|------|
| Language | Go (Golang) |
| Containerization | Docker |
| CI | GitHub Actions |
| CD (GitOps) | Argo CD |
| Orchestration | Kubernetes |
| Cloud Provider | AWS |
| Kubernetes Platform | Amazon EKS |
| Packaging | Helm |
| Environments | Dev / QA / Prod |
| Ingress | Kubernetes Ingress Controller |
| Exposure | AWS Load Balancer + DNS |

---

## ⚙️ How the Application Works

- The Go application starts an HTTP server
- It exposes web endpoints (example: `/`)
- The app runs inside a Docker container
- Configuration is injected via environment variables
- The container is deployed as a Kubernetes **Deployment**
- Traffic is routed through:
Ingress → Load Balancer → DNS → Application


---

## 🔁 CI/CD Workflow (Step-by-Step)

### 🔹 Continuous Integration (CI)
Triggered on every push to the repository:

1. GitHub Actions workflow starts
2. Docker image is built
3. Image is tagged (commit hash / version)
4. Image is pushed to container registry
5. Helm `values.yaml` is updated with the new image tag
6. Helm changes are committed back to Git

---

### 🔹 Continuous Deployment (CD – GitOps)

1. Argo CD watches the Helm repository
2. Detects changes in image version
3. Automatically syncs the application
4. Deploys the new version to EKS
5. Kubernetes performs rolling update with zero downtime

---

## 📦 Helm & Environments

The project uses **Helm** to manage multiple environments:

helm/
├── dev/
├── qa/
└── prod/


Each environment has:
- Separate values
- Independent releases
- Isolated configuration

This enables **safe promotion** from Dev → QA → Prod.

---

## 🌐 Kubernetes Resources Used

- **Deployment** – Runs Go application pods
- **Service** – Internal service exposure
- **Ingress** – External HTTP access
- **Ingress Controller** – Routes traffic
- **AWS Load Balancer** – Public entry point
- **DNS** – Domain mapping

---

## 🔐 Production Best Practices Implemented

✔ GitOps deployment model  
✔ Immutable Docker images  
✔ Version-tagged releases  
✔ Environment separation  
✔ Automated rollouts  
✔ Cloud-native networking  

---

## 🚧 Future Improvements

- Add `/healthz` and `/readyz` endpoints
- Add structured logging (Zap / Logrus)
- Add Prometheus & Grafana monitoring
- Add HPA (Horizontal Pod Autoscaler)
- Enable TLS with cert-manager
- Add security scanning in CI pipeline

---

## 📂 Repository Structure (Example)

.
├── main.go
├── Dockerfile
├── .github/workflows/ci.yml
├── helm/
│ ├── dev/
│ ├── qa/
│ └── prod/
└── README.md


---

## 🔗 Repository Link

👉 **GitHub:** https://github.com/shilemon/go-web-app

---

## 🙌 Author

**Emon Shil**  
DevOps | Cloud | Kubernetes | GitOps  

If you find this project helpful, feel free to ⭐ the repo or connect with me on LinkedIn.

---

## 📣 Keywords

`DevOps` `Kubernetes` `AWS EKS` `GitOps` `Argo CD` `Helm` `Docker` `CI/CD` `GoLang`

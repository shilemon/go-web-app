# Go Web Application with Enterprise-Grade DevOps Pipeline

[![Go](https://img.shields.io/badge/Go-1.21+-00ADD8?style=flat&logo=go)](https://golang.org/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28+-326CE5?style=flat&logo=kubernetes)](https://kubernetes.io/)
[![Argo CD](https://img.shields.io/badge/Argo_CD-GitOps-EF7B4D?style=flat&logo=argo)](https://argoproj.github.io/)
[![AWS EKS](https://img.shields.io/badge/AWS-EKS-FF9900?style=flat&logo=amazon-aws)](https://aws.amazon.com/eks/)

A production-ready demonstration of modern DevOps practices implementing a complete CI/CD pipeline with GitOps methodology for deploying a Go web application to Amazon EKS.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [CI/CD Pipeline](#cicd-pipeline)
- [Deployment Guide](#deployment-guide)
- [Environment Management](#environment-management)
- [Monitoring and Observability](#monitoring-and-observability)
- [Security Considerations](#security-considerations)
- [Troubleshooting](#troubleshooting)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This project demonstrates enterprise-grade DevOps practices for deploying cloud-native applications. It showcases a complete automated deployment pipeline from source code to production, incorporating industry best practices for containerization, orchestration, and continuous deployment.

**Key Features:**
- ✅ Fully automated CI/CD pipeline
- ✅ GitOps-based deployment with Argo CD
- ✅ Multi-environment support (Dev, QA, Production)
- ✅ Infrastructure as Code using Helm charts
- ✅ Zero-downtime deployments with rolling updates
- ✅ Cloud-native architecture on AWS EKS
- ✅ Production-ready security and networking

## 🏗️ Architecture

### High-Level Architecture

```
┌──────────────┐     ┌─────────────────┐     ┌──────────────────┐
│              │     │                 │     │                  │
│  Developer   │────▶│  GitHub Repo    │────▶│ GitHub Actions   │
│              │     │                 │     │  (CI Pipeline)   │
└──────────────┘     └─────────────────┘     └────────┬─────────┘
                                                       │
                                                       ▼
                                             ┌──────────────────┐
                                             │ Container Registry│
                                             │  (Docker Image)   │
                                             └────────┬─────────┘
                                                      │
                                                      ▼
                     ┌────────────────────────────────────────────┐
                     │           Argo CD (GitOps Engine)          │
                     │         Monitors Helm Repository           │
                     └────────────────┬───────────────────────────┘
                                      │
                                      ▼
                     ┌────────────────────────────────────────────┐
                     │          Amazon EKS Cluster                │
                     │  ┌──────────────────────────────────────┐  │
                     │  │  Ingress Controller                  │  │
                     │  │         ▼                            │  │
                     │  │  Kubernetes Services                 │  │
                     │  │         ▼                            │  │
                     │  │  Application Pods (Go Web App)       │  │
                     │  └──────────────────────────────────────┘  │
                     └────────────────┬───────────────────────────┘
                                      │
                                      ▼
                     ┌────────────────────────────────────────────┐
                     │      AWS Application Load Balancer         │
                     └────────────────┬───────────────────────────┘
                                      │
                                      ▼
                              ┌───────────────┐
                              │   End Users   │
                              └───────────────┘
```

### Deployment Flow

1. **Code Commit**: Developer pushes code to GitHub
2. **CI Trigger**: GitHub Actions workflow initiates
3. **Build**: Docker image is built and tagged
4. **Push**: Image pushed to container registry
5. **Update**: Helm chart values updated with new image tag
6. **GitOps Sync**: Argo CD detects changes in Git repository
7. **Deploy**: Argo CD synchronizes state to EKS cluster
8. **Expose**: Application accessible via Load Balancer and DNS

## 🛠️ Technology Stack

| **Component**           | **Technology**              | **Purpose**                           |
|-------------------------|-----------------------------|---------------------------------------|
| **Application**         | Go 1.21+                    | Backend web application               |
| **Containerization**    | Docker                      | Application packaging                 |
| **Container Registry**  | Docker Hub / ECR            | Image storage                         |
| **CI Platform**         | GitHub Actions              | Continuous Integration                |
| **CD Platform**         | Argo CD                     | GitOps-based Continuous Deployment    |
| **Orchestration**       | Kubernetes 1.28+            | Container orchestration               |
| **Cloud Provider**      | Amazon Web Services (AWS)   | Infrastructure hosting                |
| **Managed Kubernetes**  | Amazon EKS                  | Production Kubernetes cluster         |
| **Package Manager**     | Helm 3                      | Kubernetes application packaging      |
| **Ingress Controller**  | NGINX Ingress Controller    | HTTP/HTTPS routing                    |
| **Load Balancer**       | AWS Application Load Balancer | External traffic distribution       |

## 📋 Prerequisites

Before you begin, ensure you have the following tools installed:

### Required Tools

1. **kubectl** (v1.28+)
   - Command-line tool for Kubernetes cluster management
   - [Installation Guide](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)

2. **eksctl** (latest version)
   - CLI tool for Amazon EKS cluster management
   - [Installation Guide](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)

3. **AWS CLI** (v2)
   - Command-line interface for AWS services
   - [Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
   - Configure with: `aws configure`

4. **Helm** (v3+)
   - Kubernetes package manager
   - [Installation Guide](https://helm.sh/docs/intro/install/)

5. **Docker** (optional, for local development)
   - Container runtime
   - [Installation Guide](https://docs.docker.com/get-docker/)

### AWS Account Requirements

- Active AWS account with appropriate permissions
- IAM user with EKS, EC2, and VPC permissions
- AWS credentials configured locally

## 🚀 Quick Start

### Local Development

Run the application locally:

```bash
# Clone the repository
git clone https://github.com/shilemon/go-web-app.git
cd go-web-app

# Run the Go application
go run main.go

# Access the application
# Navigate to http://localhost:8080/courses in your browser
```

### Docker Development

```bash
# Build the Docker image
docker build -t go-web-app:local .

# Run the container
docker run -p 8080:8080 go-web-app:local

# Access at http://localhost:8080
```

## 📁 Project Structure

```
.
├── main.go                          # Go application entry point
├── Dockerfile                       # Container image definition
├── static/                          # Static assets (CSS, images, etc.)
│   └── images/
│       └── golang-website.png
├── .github/
│   └── workflows/
│       └── ci.yml                   # GitHub Actions CI pipeline
├── helm/                            # Helm charts directory
│   ├── Chart.yaml                   # Helm chart metadata
│   ├── values.yaml                  # Default values
│   ├── templates/                   # Kubernetes manifests
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── ingress.yaml
│   ├── dev/                         # Development environment
│   │   └── values.yaml
│   ├── qa/                          # QA environment
│   │   └── values.yaml
│   └── prod/                        # Production environment
│       └── values.yaml
└── README.md                        # Project documentation
```

## 🔄 CI/CD Pipeline

### Continuous Integration (GitHub Actions)

The CI pipeline is triggered on every push to the repository:

```yaml
# Workflow Steps:
1. Checkout code
2. Set up Go environment
3. Run tests and linting
4. Build Docker image
5. Tag image with commit SHA
6. Push image to container registry
7. Update Helm chart with new image tag
8. Commit and push Helm changes
```

**Pipeline Configuration**: `.github/workflows/ci.yml`

### Continuous Deployment (Argo CD)

GitOps-based deployment flow:

1. **Watch**: Argo CD monitors the Git repository for changes
2. **Detect**: Changes in Helm values are detected
3. **Sync**: Argo CD synchronizes desired state with cluster state
4. **Deploy**: New version is deployed with rolling update strategy
5. **Verify**: Health checks ensure successful deployment

## 📦 Deployment Guide

### Step 1: Create EKS Cluster

```bash
# Create a new EKS cluster
eksctl create cluster \
  --name demo-cluster \
  --region us-east-1 \
  --nodegroup-name standard-workers \
  --node-type t3.medium \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --managed

# Verify cluster access
kubectl get nodes
```

### Step 2: Install NGINX Ingress Controller

```bash
# Deploy NGINX Ingress Controller for AWS
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.11.1/deploy/static/provider/aws/deploy.yaml

# Verify installation
kubectl get pods -n ingress-nginx

# Get Load Balancer URL
kubectl get svc -n ingress-nginx
```

### Step 3: Install Argo CD

```bash
# Create Argo CD namespace
kubectl create namespace argocd

# Install Argo CD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Expose Argo CD UI via Load Balancer
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

# For Windows PowerShell, use:
kubectl patch svc argocd-server -n argocd -p '{\"spec\": {\"type\": \"LoadBalancer\"}}'

# Get Argo CD Load Balancer URL
kubectl get svc argocd-server -n argocd

# Retrieve initial admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### Step 4: Configure Argo CD Application

1. Access Argo CD UI using the Load Balancer URL
2. Login with username `admin` and the retrieved password
3. Create a new application pointing to your Helm chart repository
4. Configure sync policies (automatic or manual)

### Step 5: Deploy Application

```bash
# Using Helm (manual deployment)
helm install go-web-app ./helm -n production --create-namespace

# Or let Argo CD handle deployment (recommended)
# Argo CD will automatically sync and deploy based on Git repository
```

## 🌍 Environment Management

This project supports multiple isolated environments:

### Environment Strategy

| **Environment** | **Purpose**                | **Auto-Sync** | **Approval Required** |
|-----------------|----------------------------|---------------|-----------------------|
| **Development** | Feature testing            | ✅ Enabled    | ❌ No                 |
| **QA**          | Quality assurance testing  | ✅ Enabled    | ❌ No                 |
| **Production**  | Live user-facing deployment| ❌ Disabled   | ✅ Yes                |

### Environment Configuration

Each environment has its own `values.yaml` file:

```bash
# Deploy to Development
helm install go-web-app ./helm -f helm/dev/values.yaml -n dev

# Deploy to QA
helm install go-web-app ./helm -f helm/qa/values.yaml -n qa

# Deploy to Production
helm install go-web-app ./helm -f helm/prod/values.yaml -n production
```

### Promotion Strategy

```
Development → QA → Production
(Automated)   (Automated)   (Manual Approval)
```

## 📊 Monitoring and Observability

### Planned Integrations

- **Metrics**: Prometheus for metric collection
- **Visualization**: Grafana dashboards
- **Logging**: ELK Stack or CloudWatch
- **Tracing**: Jaeger or AWS X-Ray
- **Alerts**: AlertManager

### Health Checks (Roadmap)

```go
// Planned endpoints
GET /healthz    // Liveness probe
GET /readyz     // Readiness probe
GET /metrics    // Prometheus metrics
```

## 🔒 Security Considerations

### Implemented Security Measures

- ✅ Immutable Docker images
- ✅ Least privilege IAM roles
- ✅ Network policies (planned)
- ✅ Secret management via Kubernetes Secrets
- ✅ Private container registry support

### Planned Security Enhancements

- [ ] TLS/SSL with cert-manager
- [ ] Image vulnerability scanning (Trivy/Snyk)
- [ ] Pod Security Policies
- [ ] Network segmentation
- [ ] RBAC fine-tuning
- [ ] Security scanning in CI pipeline

## 🐛 Troubleshooting

### Common Issues

**Issue**: Argo CD not syncing
```bash
# Force manual sync
argocd app sync go-web-app

# Check application status
argocd app get go-web-app
```

**Issue**: Pods not starting
```bash
# Check pod status
kubectl get pods -n production

# View pod logs
kubectl logs <pod-name> -n production

# Describe pod for events
kubectl describe pod <pod-name> -n production
```

**Issue**: Ingress not working
```bash
# Verify Ingress Controller
kubectl get pods -n ingress-nginx

# Check Ingress resource
kubectl get ingress -n production
kubectl describe ingress <ingress-name> -n production
```

### Cleanup

```bash
# Delete the EKS cluster
eksctl delete cluster --name demo-cluster --region us-east-1

# This will remove all associated resources including:
# - Worker nodes
# - VPC and networking
# - Load balancers
# - EBS volumes
```

## 🗺️ Roadmap

### Upcoming Features

- [ ] **Health Endpoints**: Add `/healthz` and `/readyz` endpoints
- [ ] **Structured Logging**: Implement Zap or Logrus
- [ ] **Observability**: Integrate Prometheus and Grafana
- [ ] **Auto-scaling**: Configure Horizontal Pod Autoscaler (HPA)
- [ ] **TLS/HTTPS**: Enable TLS with cert-manager
- [ ] **Security**: Add container image scanning
- [ ] **Database**: Add PostgreSQL integration example
- [ ] **Caching**: Implement Redis caching layer
- [ ] **Testing**: Increase test coverage and add integration tests
- [ ] **Documentation**: Add API documentation with Swagger

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

Please ensure your code follows Go best practices and includes appropriate tests.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Emon Shil**

- **Role**: DevOps Engineer | Cloud Architect
- **Specialization**: Kubernetes, AWS, GitOps, CI/CD
- **GitHub**: [@shilemon](https://github.com/shilemon)
- **LinkedIn**: [Connect with me](https://www.linkedin.com/in/emon-shil)

---

## 📚 Additional Resources

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Amazon EKS Best Practices](https://aws.github.io/aws-eks-best-practices/)
- [Argo CD Documentation](https://argo-cd.readthedocs.io/)
- [Helm Documentation](https://helm.sh/docs/)
- [Go Documentation](https://golang.org/doc/)

## 🏷️ Tags

`DevOps` `Kubernetes` `AWS` `EKS` `GitOps` `Argo-CD` `Helm` `Docker` `CI-CD` `GitHub-Actions` `Golang` `Infrastructure-as-Code` `Cloud-Native` `Microservices` `Continuous-Deployment`

---

⭐ **If you found this project helpful, please consider giving it a star!**

# 🚀 Go Web App – End-to-End DevOps CI/CD with GitOps on AWS EKS

This project demonstrates a **production-grade DevOps pipeline** for a **Go web application**, implementing **CI/CD, GitOps, Kubernetes, Helm, and AWS EKS** best practices.

The goal of this project is to show **how modern cloud-native applications are built, packaged, deployed, and exposed automatically** from code push to production.

---

## 🧠 Project Overview

This is a simple **Go-based HTTP web application** that runs inside a Docker container and is deployed to a **Kubernetes cluster (Amazon EKS)** using **Helm and Argo CD**.

The entire deployment lifecycle is automated using **GitHub Actions (CI)** and **Argo CD (CD)** following **GitOps principles**.

## 🏗️ Architecture Diagram

<img width="1530" height="736" alt="diagram-export-2-3-2026-11_34_31-AM" src="https://github.com/user-attachments/assets/37215e22-8801-413c-8a61-49358ce7488c" />

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


# Install Argo CD

## Install Argo CD using manifests

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Access the Argo CD UI (Loadbalancer service) 

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
## Access the Argo CD UI (Loadbalancer service) -For Windows

```bash
kubectl patch svc argocd-server -n argocd -p '{\"spec\": {\"type\": \"LoadBalancer\"}}'
```

## Get the Loadbalancer service IP

```bash
kubectl get svc argocd-server -n argocd
```

For EKS prerequisite 
# prerequisites

kubectl – A command line tool for working with Kubernetes clusters. For more information, see [Installing or updating kubectl]("https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html").

eksctl – A command line tool for working with EKS clusters that automates many individual tasks. For more information, see [Installing or updating]("https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html").

AWS CLI – A command line tool for working with AWS services, including Amazon EKS. For more information, see [Installing, updating, and uninstalling the AWS CLI]("https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html") in the AWS Command Line Interface User Guide. After installing the AWS CLI, we recommend that you also configure it. For more information, see [Quick configuration]("https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config") with aws configure in the AWS Command Line Interface User Guide.


# Install EKS

Please follow the prerequisites doc before this.

## Install a EKS cluster with EKSCTL

```
eksctl create cluster --name demo-cluster --region us-east-1 
```

## Delete the cluster

```
eksctl delete cluster --name demo-cluster --region us-east-1
```

# Install Nginx Ingress Controller on AWS

## Step 1: Deploy the below manifest

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.11.1/deploy/static/provider/aws/deploy.yaml
```


<img width="1915" height="963" alt="Screenshot 2026-02-03 011020" src="https://github.com/user-attachments/assets/c689f8c6-4674-4c4e-9d65-b935b08f8c87" />
<img width="1919" height="856" alt="Screenshot 2026-02-03 011030" src="https://github.com/user-attachments/assets/d55f59a3-ec12-41c9-bf24-7d1d5ed34efb" />
<img width="1911" height="895" alt="Screenshot 2026-02-03 011043" src="https://github.com/user-attachments/assets/37170325-212e-4e96-9a6c-a2ef03e7bdb6" />

<img width="1657" height="570" alt="Screenshot 2026-02-03 011133" src="https://github.com/user-attachments/assets/1523cd02-2346-4ed8-bd48-272c1b9c2001" />

<img width="1913" height="974" alt="Screenshot 2026-02-03 011303" src="https://github.com/user-attachments/assets/f0b9d8d1-fa31-4352-a450-5236dae2145b" />

<img width="1918" height="1010" alt="Screenshot 2026-02-03 011339" src="https://github.com/user-attachments/assets/73c5837c-9a76-4039-8370-ee71a2d3cd5d" />

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

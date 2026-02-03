# Go Web Application

This is a simple website written in Golang. It uses the `net/http` package to serve HTTP requests.

## Running the server

To run the server, execute the following command:

```bash
go run main.go
```

The server will start on port 8080. You can access it by navigating to `http://localhost:8080/courses` in your web browser.

## Looks like this

![Website](static/images/golang-website.png)


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

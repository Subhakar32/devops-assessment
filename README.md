# 🐳 DevOps Internship Assessment  
**Project:** Containerize and Deploy a Next.js Application using Docker, GitHub Actions, and Minikube  

---

## 📘 Overview  
This project demonstrates a complete DevOps pipeline to:  
- Containerize a **Next.js** application using **Docker**  
- Automate image builds and pushes using **GitHub Actions** and **GitHub Container Registry (GHCR)**  
- Deploy the containerized app on **Kubernetes (Minikube)** using declarative manifests  

---

## 🧱 Project Structure  
├── .github/
│ └── workflows/
│ └── docker-build.yml
├── k8s/
│ ├── deployment.yaml
│ └── service.yaml
├── Dockerfile
├── package.json
├── pages/
│ └── index.js
├── public/
├── README.md
└── next.config.js

---

## ⚙️ Prerequisites  
Ensure the following are installed locally before starting:  
- [Node.js (v18+)](https://nodejs.org/)  
- [Docker](https://www.docker.com/)  
- [GitHub CLI](https://cli.github.com/)  
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)  
- [kubectl](https://kubernetes.io/docs/tasks/tools/)  

---

## 🚀 Setup Instructions  

# 1️⃣ Clone the Repository  
```bash
git clone https://github.com/Subhakar32/devops-assessment.git
cd devops-assessment
# 2️⃣ Install Dependencies
```bash
npm install
#3️⃣ Run Locally
```bash
npm run dev
The app will be accessible at 👉 http://localhost:3000
# 🐋 Docker Setup
Build Docker Image
```bash
docker build -t subhakar32/nextjs-app:latest .
Run Docker Container
```bash
docker run -p 3000:3000 subhakar32/nextjs-app:latest
The app will be accessible at 👉 http://localhost:3000
# ⚙️ GitHub Actions Workflow
File: .github/workflows/docker-build.yml
name: Build and Push Docker Image
```yaml
on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Login to GitHub Container Registry
        uses: docker/login-action@v2
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and Push Image
        uses: docker/build-push-action@v4
        with:
          push: true
          tags: ghcr.io/${{ github.repository_owner }}/nextjs-app:latest
This workflow automatically builds and pushes the Docker image to GHCR whenever code is pushed to the main branch.

# ☸️ Kubernetes Deployment (Minikube)
Start Minikube
```bash
minikube start
Apply Manifests
```bash
kubectl apply -f k8s/
Verify Deployment
```bash
kubectl get pods
kubectl get svc
Access the Application
```bash
minikube service nextjs-service
# 🧩 Kubernetes Manifests
deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nextjs-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nextjs-app
  template:
    metadata:
      labels:
        app: nextjs-app
    spec:
      containers:
      - name: nextjs
        image: ghcr.io/subhakar32/nextjs-app:latest
        ports:
        - containerPort: 3000
        livenessProbe:
          httpGet:
            path: /
            port: 3000
          initialDelaySeconds: 10
          periodSeconds: 10
service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nextjs-service
spec:
  type: NodePort
  selector:
    app: nextjs-app
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000

# 📦 Repository & Image URLs

GitHub Repository: https://github.com/Subhakar32/devops-assessment

GHCR Image URL: ghcr.io/subhakar32/nextjs-app:latest
# 🧮 Evaluation Focus
| Area                 | Description                                          |
| -------------------- | ---------------------------------------------------- |
| 🐳 **Docker**        | Multi-stage, optimized image builds                  |
| ⚙️ **CI/CD**         | GitHub Actions automation with GHCR integration      |
| ☸️ **Kubernetes**    | Deployment, service configuration, and health checks |
| 📖 **Documentation** | Clear, reproducible setup and deployment guide       |

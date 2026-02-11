markdown
# 🛍️ QualiBytesShop – Modern E-Commerce Platform

![Next.js](https://img.shields.io/badge/Next.js-14-black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue)
![MongoDB](https://img.shields.io/badge/MongoDB-8.1-green)
![Redux](https://img.shields.io/badge/Redux-Toolkit-purple)
![AWS](https://img.shields.io/badge/AWS-EKS-orange)
![Terraform](https://img.shields.io/badge/Terraform-IaC-623CE4)
![License](https://img.shields.io/badge/License-MIT-yellow)

QualiBytesShop is a **production-grade full-stack e-commerce platform** built with **Next.js 14, TypeScript, MongoDB**, and deployed using **AWS, Terraform, Kubernetes (EKS), Jenkins, and Argo CD**.

This project demonstrates a complete **CI/CD + GitOps + Cloud Infrastructure pipeline**.

---

# 📌 Table of Contents

- [✨ Features](#-features)
- [🏗 Architecture](#-architecture)
- [🛠 Tech Stack](#-tech-stack)
- [✅ Prerequisites](#-prerequisites)
- [🚀 Infrastructure Setup (Terraform + AWS)](#-infrastructure-setup-terraform--aws)
- [🔧 Jenkins CI Setup](#-jenkins-ci-setup)
- [🔄 Continuous Deployment (Argo CD)](#-continuous-deployment-argo-cd)
- [🌐 Ingress & HTTPS Setup](#-ingress--https-setup)
- [🎉 Final Deployment](#-final-deployment)
- [📄 License](#-license)

---

# ✨ Features

- 🎨 Modern responsive UI (Dark/Light mode)
- 🔐 JWT-based Authentication
- 🛒 Real-time Cart Management (Redux)
- 📱 Mobile-first Design
- 🔍 Advanced Search & Filtering
- 💳 Secure Checkout
- 👤 User Profiles & Order History
- ☁ Production Deployment on AWS EKS
- 🔁 CI/CD using Jenkins
- 🚀 GitOps Deployment with Argo CD
- 🔒 HTTPS with Cert-Manager & Nginx Ingress

---

# 🏗 Architecture

This project follows a **Three-Tier Architecture**:

## 1️⃣ Presentation Tier (Frontend)
- Next.js 14
- React Components
- Redux Toolkit
- Tailwind CSS

## 2️⃣ Application Tier (Backend)
- Next.js API Routes
- Authentication & Authorization
- Business Logic
- Request Validation
- Error Handling

## 3️⃣ Data Tier (Database)
- MongoDB
- Mongoose ODM
- Data Models
- CRUD Operations

---

# 🛠 Tech Stack

|
 Layer 
|
 Technology 
|
|
--------
|
------------
|
|
 Frontend 
|
 Next.js, TypeScript, Tailwind 
|
|
 Backend 
|
 Node.js (API Routes) 
|
|
 Database 
|
 MongoDB 
|
|
 CI 
|
 Jenkins 
|
|
 CD 
|
 Argo CD 
|
|
 Container 
|
 Docker 
|
|
 Orchestration 
|
 Kubernetes (EKS) 
|
|
 IaC 
|
 Terraform 
|
|
 Cloud 
|
 AWS 
|

---

# ✅ Prerequisites

Ensure the following tools are installed:

- Terraform
- AWS CLI
- kubectl
- Helm
- Docker
- Git

---

# 🚀 Infrastructure Setup (Terraform + AWS)

---

## 1️⃣ Install Terraform

### Linux / macOS

```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
Verify:

bash
terraform -v
Initialize:

bash
terraform init
2️⃣ Install AWS CLI
bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
Configure AWS:

bash
aws configure
3️⃣ Clone Repository
bash
git clone https://github.com/Satyams-git/Qualibytes-Ecommerce
cd terraform
4️⃣ Generate SSH Key
bash
ssh-keygen -f qualibytes-key
chmod 400 qualibytes-key
5️⃣ Deploy Infrastructure
bash
terraform init
terraform plan
terraform apply
6️⃣ Connect to EC2
bash
ssh -i qualibytes-key ubuntu@<public-ip>
7️⃣ Update kubeconfig
bash
aws eks --region ap-south-1 update-kubeconfig --name qualibytes-eks-cluster
kubectl get nodes
🔧 Jenkins CI Setup
Start Jenkins
bash
sudo systemctl enable jenkins
sudo systemctl restart jenkins
Check status:

bash
sudo systemctl status jenkins
Get initial password:

bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Access:

text
http://<public-ip>:8080
Required Plugins
Docker Pipeline
Pipeline View
Add Credentials
GitHub Credentials
Kind: Username with password
ID: github-credentials
DockerHub Credentials
Kind: Username with password
ID: docker-hub-credentials
Setup Shared Library
Name: shared
Branch: main
Repository: jenkins-shared-libraries
Create Pipeline Job
Name: qbShop
Type: Pipeline
SCM: Git
Script Path: Jenkinsfile
Enable GitHub Webhook trigger
🔄 Continuous Deployment (Argo CD)
Install Argo CD
bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Check pods:

bash
kubectl get pods -n argocd
Change Service to NodePort
bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
Get Admin Password
bash
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d; echo
Login:

Username: admin
🌐 Ingress & HTTPS Setup
Install Nginx Ingress
bash
kubectl create namespace ingress-nginx
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.service.type=LoadBalancer
Install Cert-Manager
bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --set installCRDs=true
Apply HTTPS Manifests
bash
kubectl apply -f 00-cluster-issuer.yaml
kubectl apply -f 04-configmap.yaml
kubectl apply -f 10-ingress.yaml
Verify:

bash
kubectl get certificate -n qbshop
kubectl describe certificate qbshop-tls -n qbshop
🎉 Final Deployment
Your application is now deployed with:

✅ AWS EKS
✅ Terraform Infrastructure
✅ Docker Containers
✅ Jenkins CI
✅ Argo CD GitOps
✅ Nginx Ingress
✅ HTTPS via Cert-Manager

Production-ready architecture 🚀

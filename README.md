markdown
# 🛍️ QualiBytesShop - Modern E-commerce Platform

![Next.js](https://img.shields.io/badge/Next.js-14-black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue)
![MongoDB](https://img.shields.io/badge/MongoDB-8.1-green)
![Redux](https://img.shields.io/badge/Redux-Toolkit-purple)
![License](https://img.shields.io/badge/License-MIT-yellow)

QualiBytesShop is a modern, full-stack e-commerce platform built with **Next.js 14**, **TypeScript**, and **MongoDB**.  
It features a beautiful UI with Tailwind CSS, secure authentication, real-time cart updates, and a seamless shopping experience.

---

# ✨ Features

- 🎨 Modern and responsive UI with dark mode support  
- 🔐 Secure JWT-based authentication  
- 🛒 Real-time cart management with Redux  
- 📱 Mobile-first design  
- 🔍 Advanced product search & filtering  
- 💳 Secure checkout process  
- 📦 Multiple product categories  
- 👤 User profiles & order history  
- 🌙 Dark / Light theme support  

---

# 🏗️ Architecture

QualiBytesShop follows a **Three-Tier Architecture**:

## 1️⃣ Presentation Tier (Frontend)

- Next.js React Components  
- Redux (State Management)  
- Tailwind CSS  
- Client-side Routing  
- Responsive UI Components  

## 2️⃣ Application Tier (Backend)

- Next.js API Routes  
- Business Logic  
- Authentication & Authorization  
- Request Validation  
- Error Handling  
- Data Processing  

## 3️⃣ Data Tier (Database)

- MongoDB  
- Mongoose ODM  
- Data Models  
- CRUD Operations  
- Data Validation  

---

# ✅ Prerequisites

Before setting up this project, make sure the following tools are installed:

- Terraform
- AWS CLI
- kubectl
- Helm
- Docker
- Git

---

# ⚙️ Infrastructure Setup (Terraform + AWS)

---

## 1️⃣ Install Terraform

### Linux / macOS

```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
Verify Installation
bash
terraform -v
Initialize Terraform
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
You will be prompted for:

AWS Access Key ID
AWS Secret Access Key
Default region
Output format
⚠️ Ensure your IAM user has proper permissions.

🚀 Getting Started
Clone Repository
bash
git clone https://github.com/Satyams-git/Qualibytes-Ecommerce
cd terraform
Generate SSH Key
bash
ssh-keygen -f qualibytes-key
chmod 400 qualibytes-key
Terraform Deployment
bash
terraform init
terraform plan
terraform apply
Confirm with yes.

Access EC2 Instance
bash
ssh -i qualibytes-key ubuntu@<public-ip>
Update kubeconfig
bash
aws configure
aws eks --region ap-south-1 update-kubeconfig --name qualibytes-eks-cluster
kubectl get nodes
🔧 Jenkins Setup
Check Jenkins Status
bash
sudo systemctl status jenkins
Start Jenkins
bash
sudo systemctl enable jenkins
sudo systemctl restart jenkins
Get Initial Admin Password
bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Open in browser:

text
http://<public-ip>:8080
Install Required Plugins
Docker Pipeline
Pipeline View
Add Credentials (Global)
GitHub Credentials
Kind: Username with password
ID: github-credentials
DockerHub Credentials
Kind: Username with password
ID: docker-hub-credentials
Setup Shared Library
Name: shared
Branch: main
Repo: https://github.com/<your-username>/jenkins-shared-libraries
Create Pipeline Job
Name: qbShop
Type: Pipeline
SCM: Git
Script Path: Jenkinsfile
Enable GitHub Webhook Trigger
🔄 Continuous Deployment (CD)
SSH into Bastion Server:

bash
aws configure
aws eks update-kubeconfig --region eu-west-1 --name qualibytes-eks-cluster
🚀 Argo CD Setup
Create Namespace
bash
kubectl create namespace argocd
Install Argo CD
bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Check Pods:

bash
kubectl get pods -n argocd
Change Service to NodePort:

bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
Get Admin Password:

bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
Login:

Username: admin
Password: (decoded password)
🌐 Nginx Ingress Controller
bash
kubectl create namespace ingress-nginx
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.service.type=LoadBalancer
🔐 Install Cert-Manager
bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.12.0 \
  --set installCRDs=true
🌍 DNS Setup
Get LoadBalancer DNS:

bash
kubectl get svc -n ingress-nginx
Add CNAME record in GoDaddy pointing to LoadBalancer hostname.

🔒 Enable HTTPS
Apply Manifests
bash
kubectl apply -f 00-cluster-issuer.yaml
kubectl apply -f 04-configmap.yaml
kubectl apply -f 10-ingress.yaml
Verify Certificate
bash
kubectl get certificate -n qbshop
kubectl describe certificate qbshop-tls -n qbshop
kubectl get challenges -n qbshop
🎉 Congratulations!
Your QualiBytesShop application is now fully deployed with:

✅ CI/CD using Jenkins
✅ GitOps using Argo CD
✅ Kubernetes (EKS)
✅ Nginx Ingress
✅ HTTPS via Cert-Manager
✅ Production-grade Infrastructure

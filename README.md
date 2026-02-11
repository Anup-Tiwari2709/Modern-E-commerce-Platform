# Modern-E-commerce-Platform
It is a modern, full stack e-commerce platform
🛍️ QualiBytesShop - Modern E-commerce Platform
Next.js TypeScript MongoDB Redux License

QualiBytesShop is a modern, full-stack e-commerce platform built with Next.js 14, TypeScript, and MongoDB. It features a beautiful UI with Tailwind CSS, secure authentication, real-time cart updates, and a seamless shopping experience.

✨ Features
🎨 Modern and responsive UI with dark mode support
🔐 Secure JWT-based authentication
🛒 Real-time cart management with Redux
📱 Mobile-first design approach
🔍 Advanced product search and filtering
💳 Secure checkout process
📦 Multiple product categories
👤 User profiles and order history
🌙 Dark/Light theme support
🏗️ Architecture
QualiBytesShop follows a three-tier architecture pattern:

1. Presentation Tier (Frontend)
Next.js React Components
Redux for State Management
Tailwind CSS for Styling
Client-side Routing
Responsive UI Components
2. Application Tier (Backend)
Next.js API Routes
Business Logic
Authentication & Authorization
Request Validation
Error Handling
Data Processing
3. Data Tier (Database)
MongoDB Database
Mongoose ODM
Data Models
CRUD Operations
Data Validation
PreRequisites
Important

Before you begin setting up this project, make sure the following tools are installed and configured properly on your system:

Setup & Initialization
1. Install Terraform
Install Terraform
Linux & macOS
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
Verify Installation
terraform -v
Initialize Terraform
terraform init
2. Install AWS CLI
AWS CLI (Command Line Interface) allows you to interact with AWS services directly from the command line.

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
aws configure

This will prompt you to enter:
AWS Access Key ID:
AWS Secret Access Key:
Default region name:
Default output format:
Note

Make sure the IAM user you're using has the necessary permissions. You’ll need an AWS IAM Role with programmatic access enabled, along with the Access Key and Secret Key.

Getting Started
Follow the steps below to get your infrastructure up and running using Terraform:

Clone the Repository: First, clone this repo to your local machine:
git clone https://github.com/Satyams-git/Qualibytes-Ecommerce
cd terraform
Generate SSH Key Pair: Create a new SSH key to access your EC2 instance:
ssh-keygen -f qualibytes-key
This will prompt you to create a new key file named qualibytes-key.

Private key permission: Change your private key permission:
chmod 400 qualibytes-key
Initialize Terraform: Initialize the Terraform working directory to download required providers:
terraform init
Review the Execution Plan: Before applying changes, always check the execution plan:
terraform plan
Apply the Configuration: Now, apply the changes and create the infrastructure:
terraform apply
Confirm with yes when prompted.

Access Your EC2 Instance;
After deployment, grab the public IP of your EC2 instance from the output or AWS Console, then connect using SSH:
ssh -i qualibytes-key ubuntu@<public-ip>
Update your kubeconfig: wherever you want to access your eks wheather it is yur local machine or bastion server this command will help you to interact with your eks.
Caution

you need to configure aws cli first to execute this command:

aws configure
aws eks --region ap-south-1 update-kubeconfig --name qualibytes-eks-cluster
Check your cluster:
kubectl get nodes
Jenkins Setup Steps
Tip

Check if jenkins service is running:

sudo systemctl status jenkins
Steps to Access Jenkins & Install Plugins
1. Open Jenkins in Browser:
Use your public IP with port 8080: http://<public_IP>:8080

2. Initial Admin password:
Start the service and get the Jenkins initial admin password:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword
3. Start Jenkins (If Not Running):
Get the Jenkins initial admin password:

sudo systemctl enable jenkins
sudo systemctl restart jenkins
4. Install Essential Plugins:
Navigate to: Manage Jenkins → Plugins → Available Plugins
Search and install the following:
Docker Pipeline
Pipeline View
5. Set Up Docker & GitHub Credentials in Jenkins (Global Credentials)
GitHub Credentials:
Go to: Jenkins → Manage Jenkins → Credentials → (Global) → Add Credentials
Use:
Kind: Username with password
ID: github-credentials
DockerHub Credentials: Go to the same Global Credentials section
Use:
Kind: Username with password
ID: docker-hub-credentials [Notes:] Use these IDs in your Jenkins pipeline for secure access to GitHub and DockerHub
6. Jenkins Shared Library Setup:
Configure Trusted Pipeline Library:

Go to: Jenkins → Manage Jenkins → Configure System Scroll to Global Pipeline Libraries section
Add a New Shared Library:

Name: shared

Default Version: main

Project Repository URL: https://github.com/<your user-name/jenkins-shared-libraries.

[Notes:] Make sure the repo contains a proper directory structure eq: vars/

7. Setup Pipeline
Create New Pipeline Job
Name: qbShop
Type: Pipeline
Press Okey
In General

Description: qbShop
Check the box: GitHub project
GitHub Repo URL: https://github.com/<your user-name/qualibytes-ecommerce-app
In Trigger

Check the box:GitHub hook trigger for GITScm polling
In Pipeline

Definition: Pipeline script from SCM
SCM: Git
Repository URL: https://github.com/<your user-name/qualibytes-ecommerce-app
Credentials: github-credentials
Branch: master
Script Path: Jenkinsfile
Fork Required Repos
Fork App Repo:

Open the Jenkinsfile
Change the DockerHub username to yours
Fork Shared Library Repo:

Edit vars/update_k8s_manifest.groovy
Update with your DockerHub username
Setup Webhook
In GitHub:

Go to Settings → Webhooks
Add a new webhook pointing to your Jenkins URL
Select: GitHub hook trigger for GITScm polling in Jenkins job
Trigger the Pipeline
Click Build Now in Jenkins

8. CD – Continuous Deployment Setup
Prerequisites:
Before configuring CD, make sure the following tools are installed:

Installations Required:
kubectl
AWS CLI
SSH into Bastion Server

Connect to your Bastion EC2 instance via SSH.
Note:
This is not the node where Jenkins is running. This is the intermediate EC2 (Bastion Host) used for accessing private resources like your EKS cluster.

8. Configure AWS CLI on Bastion Server Run the AWS configure command:

aws configure
Add your Access Key and Secret Key when prompted.

9. Update Kubeconfig for EKS
Run the following important command:

aws eks update-kubeconfig --region eu-west-1 --name qualibytes-eks-cluster
This command maps your EKS cluster with your Bastion server.
It helps to communicate with EKS components.
10. Argo CD Setup
Create a Namespace for Argo CD

kubectl create namespace argocd
Install Argo CD using Manifest
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Watch Pod Creation
watch kubectl get pods -n argocd
This helps monitor when all Argo CD pods are up and running.

Check Argo CD Services

kubectl get svc -n argocd
Change Argo CD Server Service to NodePort
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
Access Argo CD GUI
Check Argo CD Server Port (again, post NodePort change)
kubectl get svc -n argocd
Port Forward to Access Argo CD in Browser
Forward Argo CD service to access the GUI:
kubectl port-forward svc/argocd-server -n argocd <your-port>:443 --address=0.0.0.0 &
Replace with a local port of your choice (e.g., 8080).
Now, open https://: in your browser.
Get the Argo CD Admin Password

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
Log in to the Argo CD GUI
Username: admin
Password: (Use the decoded password from the previous command)
Update Your Password
On the left panel of Argo CD GUI, click on "User Info"
Select Update Password and change it.
Deploy Your Application in Argo CD GUI
On the Argo CD homepage, click on the “New App” button.
Fill in the following details:
Application Name: Enter your desired app name
Project Name: Select default from the dropdown.
Sync Policy: Choose Automatic.
In the Source section:
Repo URL: Add the Git repository URL that contains your Kubernetes manifests.
Path: Kubernetes (or the actual path inside the repo where your manifests reside)
In the “Destination” section:
Cluster URL: https://kubernetes.default.svc (usually shown as "default")
Namespace: qualibytes-ecommerce-app (or your desired namespace)
Click on “Create”.
Nginx ingress controller:
Install the Nginx Ingress Controller using Helm:
kubectl create namespace ingress-nginx
Add the Nginx Ingress Controller Helm repository:
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
Install the Nginx Ingress Controller:
helm install nginx-ingress ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.service.type=LoadBalancer
Check the status of the Nginx Ingress Controller:
kubectl get pods -n ingress-nginx
Get the external IP address of the LoadBalancer service:
kubectl get svc -n ingress-nginx
Install Cert-Manager
Jetpack: Add the Jetstack Helm repository:
helm repo add jetstack https://charts.jetstack.io
helm repo update
Cert-Manager: Install the Cert-Manager Helm chart:
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.12.0 \
  --set installCRDs=true
**Check pods:**Check the status of the Cert-Manager pods:
kubectl get pods -n cert-manager
DNS Setup: Find your DNS name from the LoadBalancer service:
kubectl get svc nginx-ingress-ingress-nginx-controller -n ingress-nginx -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
Create a DNS record for your domain pointing to the LoadBalancer IP.
Go to your godaddy dashboard and create a new CNAME record and map the DNS just your got in the terminal.
HTTPS:
1. Update your manifests to enable HTTPS:
04-configmap.yaml

apiVersion: v1
kind: ConfigMap
metadata:
  name: qbshop-config
  namespace: qbshop
data:
  MONGODB_URI: "mongodb://mongodb-service:27017/easyshop"
  NODE_ENV: "production"
  NEXT_PUBLIC_API_URL: "https://qbshop.asriv.shop/api"
  NEXTAUTH_URL: "https://qbshop.asriv.shop/"
  NEXTAUTH_SECRET: "HmaFjYZ2jbUK7Ef+wZrBiJei4ZNGBAJ5IdiOGAyQegw="
  JWT_SECRET: "e5e425764a34a2117ec2028bd53d6f1388e7b90aeae9fa7735f2469ea3a6cc8c"
2. Update your manifests to enable HTTPS:
10-ingress.yaml

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: qbshop-ingress
  namespace: qbshop
  annotations:
    nginx.ingress.kubernetes.io/proxy-body-size: "50m"
    kubernetes.io/ingress.class: "nginx"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
  - hosts:
    - qbshop.asriv.shop
    secretName: qbshop-tls
  rules:
  - host: qbshop.asriv.shop
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: qbshop-service
            port:
              number: 80
3. Apply your manifests:
kubectl apply -f 00-cluster-issuer.yaml
kubectl apply -f 04-configmap.yaml
kubectl apply -f 10-ingress.yaml
4. Commands to check the status:
kubectl get certificate -n qbshop
kubectl describe certificate qbshop-tls -n qbshop
kubectl logs -n cert-manager -l app=cert-manager
kubectl get challenges -n qbshop
kubectl describe challenges -n qbshop
Congratulations!
Your project is now deployed.

# 🎬 Tickets Management System on Azure

## 📌 Project Overview
This project is a **Tickets Management System (similar to movie tickets)** built using **.NET 7**, deployed on **Azure Kubernetes Service (AKS)** with a **fully private infrastructure** provisioned using **Terraform**.  
The project uses **Azure DevOps CI/CD** to build, push, and deploy containerized applications securely.

---

## 🏗️ Infrastructure (Terraform)

All infrastructure components are provisioned using **Terraform** on Microsoft Azure.

### 🌍 Core Resources
1. **Resource Group**  
   - Central container for all Azure resources.

2. **Virtual Network (VNET)**  
   - Provides isolated and secure networking.

3. **Subnets**
   - AKS Subnet  
   - VPN Subnet  
   - MSSQL Subnet  
   - VM & ACR Subnet  
   - Application Gateway Subnet  

---

### 🔐 Security & Private Access
4. **Private Azure SQL Server & SQL Database**
   - Azure SQL Server and Database using **Private Endpoint only**.
   - No public access enabled.

5. **Private Azure Key Vault**
   - Stores:
     - SSL Certificate for the website
     - Secrets and sensitive configuration

6. **Private DNS Zones**
   - DNS resolution for private endpoints (AKS, ACR, SQL, Key Vault).
   - Custom DNS for application domain.

---

### 🌐 Connectivity
7. **Virtual Private Network (VPN)**
   - Secure access to private Azure resources.

8. **NAT Gateway**
   - Handles **egress (outbound) traffic from AKS**.
   - Prevents exposing public IPs on AKS nodes.

---

### 🚀 Compute & Containers
9. **Private AKS (Azure Kubernetes Service)**
   - Private cluster with no public API server.
   - Accessible only through private networking.

10. **Private Azure Container Registry (ACR)**
    - Stores Docker images securely.
    - Accessible only from AKS.

11. **Private Virtual Machine**
    - Used as a **self-hosted Azure DevOps agent**.
    - No public IP address.

---

### 🌐 Application Delivery
12. **Application Gateway**
    - Acts as the AKS Ingress Controller.
    - HTTPS enabled using certificate from Azure Key Vault.

---

## 🧩 Application Layer

### 🖥️ Backend Application
- **.NET 7 Web Application**
- Responsibilities:
  - Display and manage tickets (movies/events)
  - Connect to Azure SQL Database securely

---

### 🐳 Docker
- The project includes a **Dockerfile** that:
  - Builds the .NET 7 application
  - Creates a production-ready Docker image
  - Pushes the image to Azure Container Registry (ACR)

---

## ☸️ Kubernetes Manifests

Kubernetes manifests include:

1. **Secret**
   - Stores SQL connection strings and credentials
   - Enables Pods to access **Azure SQL (PaaS)** securely

2. **Deployment**
   - Runs application Pods
   - Defines resource limits and replicas
   - Pulls images from ACR

3. **Service**
   - Internal Kubernetes service for application communication

4. **Ingress**
   - Configured with **Application Gateway Ingress Controller**
   - HTTPS with custom domain

---

## 🔄 CI/CD Pipeline (Azure DevOps)

### 📁 azure-pipelines.yml
The pipeline performs the following steps:

1. **Build Stage**
   - Build .NET 7 application
   - Build Docker image

2. **Push Stage**
   - Authenticate with Azure Container Registry
   - Push Docker image to ACR

3. **Deploy Stage**
   - Apply Kubernetes manifests
   - Deploy the application to AKS

---

## 📂 Project Structure

```bash
.
├── terraform/
│   ├── environments
│   │    ├── dev
│   │    │    ├── main.tf
│   │    │    ├── variables.tf
│   │    │    └── terraform.tfvars
│   │    │
│   │    └── prod
│   │         ├── main.tf
│   │         ├── variables.tf
│   │         └── terraform.tfvars
│   │
│   ├── modules
│        ├── Resource group
│        ├── Network + Subnets
│        ├── DNS
│        ├── Key Vault
│        ├── AKS
│        ├── ACR
│        ├── Application gateway
│        ├── NAT
│        ├── VM (azure DevOps Agent)
│        ├── VPN
│        └── Microsoft SQL server + DB
│
├── src/
│   ├── Dockerfile
│   ├── eTickets.sln
│   └── eTickets (.NET 7)
│
│
├── k8s/
│   ├── secret.yaml
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
│
├── azure-pipelines.yml
└── README.md

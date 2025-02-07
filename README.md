# Deployment Guide: Azure AKS Web Application

## Overview
This guide provides step-by-step instructions to deploy a web application in **Azure Kubernetes Service (AKS)** with best practices.

## ✅ Prerequisites
- **Azure CLI** installed ([Download](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli))
- **Terraform** installed ([Download](https://developer.hashicorp.com/terraform/downloads))
- **kubectl** installed ([Install Guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/))
- **Azure Subscription**
- **Docker installed** ([Download](https://www.docker.com/get-started))
- **Docker Hub account**

## ✅ Step 1: Deploy Azure AKS Cluster
- **Authenticate with Azure:**
   ```sh
   az login
   ```
- **Initialize and apply Terraform to provision the infrastructure:**
   ```sh
   cd terraform/
   terraform init
   terraform plan -var "subscription_id=$ARM_SUBSCRIPTION_ID" -var "tenant_id=$ARM_TENANT_ID"
   terraform apply -var "subscription_id=$ARM_SUBSCRIPTION_ID" -var "tenant_id=$ARM_TENANT_ID" -auto-approve
   ```
- **Retrieve Kubernetes credentials:**
   ```sh
   az aks get-credentials --resource-group aks-resource-group --name aks-cluster
   ```

## ✅ Step 2: Build and Push Docker Image to Docker Hub
- **Log in to Docker Hub:**
   ```sh
   docker login
   ```
- **Build and tag the Docker image:**
   ```sh
   docker build -t kishoreel/web-app:latest/web-app:latest .
   ```
- **Push the image to Docker Hub:**
   ```sh
   docker push kishoreel/web-app:latest/web-app:latest
   ```

## ✅ Step 3: Deploy the Application to AKS
- **Apply the Kubernetes deployment and service:**
   ```sh
   kubectl apply -f k8s_deployment.yaml
   ```
- **Verify pod status:**
   ```sh
   kubectl get pods
   ```
- **Check service and get external IP:**
   ```sh
   kubectl get svc web-app-service
   ```

## ✅ Step 4: Deploy Monitoring (Prometheus & Grafana)
- **Apply monitoring setup:**
   ```sh
   kubectl apply -f monitoring.yaml
   ```
- **Check running pods in the monitoring namespace:**
   ```sh
   kubectl get pods -n monitoring
   ```
- **Get external IP for Grafana:**
   ```sh
   kubectl get svc grafana-service -n monitoring
   ```
- **Access Grafana using the external IP on port 3000.**

## ✅ Step 5: Clean Up Resources (If Needed)
- **To remove the AKS cluster and all resources:**
   ```sh
   terraform destroy -auto-approve
   ```

## 🎯 Conclusion
Following these steps, you have successfully deployed a web application in **Azure Kubernetes Service (AKS)** with:
- ✅ **Terraform for Infrastructure as Code**
- ✅ **Docker for containerization**
- ✅ **Kubernetes manifests for deployment**
- ✅ **Prometheus and Grafana for monitoring**

🚀 Our application is now live and monitored efficiently!

## Drive link to access screenshots of deployment

Link: https://drive.google.com/drive/folders/1Yy53UKt7AbdQL4_ceGIURFHREuNUHndi?usp=sharing


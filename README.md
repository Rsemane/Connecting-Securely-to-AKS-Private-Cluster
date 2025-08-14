# 🔐 Securely Connecting to an AKS Private Cluster
## Overview
This guide outlines various methods for securely accessing an Azure Kubernetes Service (AKS) private cluster, particularly when the cluster is configured with a private endpoint and resides within a virtual network (VNet).

### Key Components
- **AKS Private Cluster**: A Kubernetes cluster with a public FQDN but restricted to private network access.
- **Cloud Shell in Azure Virtual Network**: Enables Cloud Shell to run inside a user-defined VNet for secure resource access.
- **Azure Container Instance (ACI)**: Hosts the Cloud Shell session when deployed into a VNet, managed by Microsoft.
- **Azure Relay**: Allows two endpoints that aren't directly reachable to communicate. It's used to allow the administrator's browser to communicate with the container in the private network.
- **Azure File Share**: Provides persistent storage for Cloud Shell containers hosted in Azure Container Instances (ACI), enabling data sharing across Cloud Shell sessions (users).

## 📘  Scenario: Accessing a Private AKS Cluster via Private Connectivity

You have an AKS private cluster with a public FQDN, but its API server is only accessible through a private endpoint (private IP) bound to a network interface (NIC) within your VNet. To interact with this cluster, you must establish connectivity to the VNet hosting the endpoint.

## ✅ Access Options

1. **Jump Server (VM) in VNet or Peered VNet**
   Deploy a virtual machine within the same VNet or a peered VNet. Ensure it is permitted by Network Security Groups (NSGs) to access the AKS endpoint.
2. **Azure Bastion via Portal**
   Use Azure Bastion to connect to a jump server VM through the Azure Portal, eliminating the need for public IPs.
3. **Cloud Shell in Azure Virtual Network**
   Launch Cloud Shell inside the VNet and use the az aks invoke or kubectl commands to interact with the AKS cluster securely.
4. **On-Premises Network via VPN or ExpressRoute**
   Connect from your corporate network to Azure using VPN or ExpressRoute, enabling secure access to the AKS private endpoint.
5. **Bastion-AKS Direct Integration (Preview)**
   Azure Bastion now supports direct integration with AKS (currently in preview). For a detailed walkthrough, refer to Richard Hooper (@PixelRobots)'s excellent explanation.

## Architecture Diagram

The diagram below presents the various access methods and infrastructure components involved in deploying and connecting to an Azure Kubernetes Service (AKS) private cluster.

![Architecture Diagram](images/Connecting%20to%20an%20AKS%20Private%20Cluster.png)


## 🚧 [Deployment Guide](Deployment.md)

This project demonstrates how to deploy Azure Cloud Shell with VNet integration to securely connect to a private AKS (Azure Kubernetes Service) cluster.

### 🔧 Key Objectives
- Enable Cloud Shell access to resources within a private virtual network.
- Establish secure connectivity to a private AKS cluster using VNet peering and private endpoints.
- Simplify developer access to Kubernetes management tools without exposing the cluster to the public internet.

## 🧰 [Resources](Resources.md)

- Microsoft Cloud Shell in Virtual Network Overview : https://learn.microsoft.com/en-us/azure/cloud-shell/vnet/overview

<img width="2857" height="1511" alt="image" src="https://github.com/user-attachments/assets/17825bfb-e3f5-4390-9d4b-662864fcd926" />

- Microsoft Cloud Shell Deployment ARM Template : https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/cloud-shell-vnet/

## ✅ Conclusion 

This deployment showcases a secure and flexible method for accessing a private AKS cluster using Azure Cloud Shell with VNet integration. By placing Cloud Shell within a virtual network, you eliminate public exposure of the Kubernetes API and ensure that all management traffic flows through trusted, internal paths.

Beyond AKS, this setup also enables secure access to other private Azure resources such as storage accounts, databases, and virtual machines; making Cloud Shell a powerful tool for managing infrastructure in isolated environments.

This approach aligns with best practices for enterprise-grade security, operational efficiency, and network segmentation in Azure.

# 🔐 Securely Connecting to an AKS Private Cluster
## Overview
This guide outlines various methods for securely accessing an Azure Kubernetes Service (AKS) private cluster, particularly when the cluster is configured with a private endpoint and resides within a virtual network (VNet).

### Key Components
- **AKS Private Cluster**: A Kubernetes cluster with a public FQDN but restricted to private network access.
- **Cloud Shell VNet Integration**: Enables Cloud Shell to run inside a user-defined VNet for secure resource access.
- **Azure Container Instance (ACI)**: Hosts the Cloud Shell session when deployed into a VNet, managed by Microsoft.

## 📘  Scenario: Accessing a Private AKS Cluster via Private Connectivity

You have an AKS private cluster with a public FQDN, but its API server is only accessible through a private endpoint (private IP) bound to a network interface (NIC) within your VNet. To interact with this cluster, you must establish connectivity to the VNet hosting the endpoint.

## ✅ Access Options

1. **Jump Server (VM) in VNet or Peered VNet**
   Deploy a virtual machine within the same VNet or a peered VNet. Ensure it is permitted by Network Security Groups (NSGs) to access the AKS endpoint.
2. **Azure Bastion via Portal**
   Use Azure Bastion to connect to a jump server VM through the Azure Portal, eliminating the need for public IPs.
3. **Cloud Shell with VNet Integration**
   Launch Cloud Shell inside the VNet and use the az aks invoke or kubectl commands to interact with the AKS cluster securely.
4. **On-Premises Network via VPN or ExpressRoute**
   Connect from your corporate network to Azure using VPN or ExpressRoute, enabling secure access to the AKS private endpoint.
5. **Bastion-AKS Direct Integration (Preview)**
   Azure Bastion now supports direct integration with AKS (currently in preview). For a detailed walkthrough, refer to Richard Hooper (@PixelRobots)'s excellent explanation.

## Architecture Diagram

The diagram below presents the various access methods and infrastructure components involved in deploying and connecting to an Azure Kubernetes Service (AKS) private cluster.

![Architecture Diagram](`images/"Connecting to an AKS Private Cluster.png"`)

# 🚀 Deployment Flow Overview
This guide outlines the steps to deploy and securely connect to an Azure Kubernetes Service (AKS) private cluster using Cloud Shell with VNet integration, following a hub-and-spoke network topology.

 ## Access Options Flow 
For the purpose of this demonstration, access to the AKS private cluster will be established using Access Option 3.   

![Architecture Diagram](images/Flow%20Connecting%20to%20an%20AKS%20Private%20Cluster.png)


## 🧱 Existing Infrastructure 
- Using a Free Azure Subscription 
- Owner role already assigned on the subscription. 
- Hub & spoke Network Topology is already configured:
    - *Hub VNet*:  Hosts Cloud Shell (VNet-integrated).
    - *Spoke VNet*: Hosts the AKS Private Cluster.
    - *VNet Peering*: Established between Hub and Spoke VNets.


## ☸️ AKS Private Cluster Deployment

- Create a Private AKS cluster with private API server endpoint.

## 🔗  Link Hub VNet (VNet where Cloud Shell will be deployed) to Private DNS Zone. 
- Establish a link between Private DNS Zone and VNet where Cloud Shell is deployed to resolve the public FQDN to Private IP.  

## <img width="77" height="85" alt="image" src="https://github.com/user-attachments/assets/e58663d2-1b30-4081-af94-cd28dec08937" />  Azure Relay Deployment & configure Cloud Shell
- Deploy Azure Relay 
- Configure Cloud Shell within the Hub VNet (or VNet of your choice that is peered to AKS VNet)
- Use Cloud Shell to securely connect to the AKS private endpoint

  
## ✅ Connection Verification
Confirm access paths:
From Portal → Cloud Shell → AKS API
From Remote Site → VNet Gateway → AKS
From Jump Box → AKS

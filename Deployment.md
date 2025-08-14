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

To deploy the AKS private cluster with a private API server endpoint, follow these steps in the Azure Portal:  

**1. Navigate to AKS Service**    
   - Go to the Azure Portal and search for Kubernetes services, then click Create.

**2. Configure Basics**
  - Fill in the required fields such as:
    -  Subscription  
    -  Resource Group  
    -  Cluster name   
    -  Region   

![Create Private AKS Cluster](images/Create%20Kubernetes%20cluster%20-%201.png)


**3. Click "Next" Until You Reach the Networking Tab**   
Proceed through the tabs (Node Pools, Authentication, etc.) until you reach Networking.   

![Create Private AKS Cluster](images/Create%20Kubernetes%20cluster%20-%202.png)


**4. Enable Private Access**  
  - In the Networking tab:
     - Check Enable private cluster (Note: The option Public access - Set authorized IP ranges will be greyed out) 
     - If you have already created an Azure Virtual Network, you may choose to use it by selecting the **Bring your own VNet** option. Otherwise, leave it unchecked to allow Azure to automatically create a new Virtual Network for the cluster. 
     - Optionally configure DNS name prefix or leave it to be auto-created.   
     - Review + Create   

![Create Private AKS Cluster](images/Create%20Kubernetes%20cluster%20-%203.png)

**5. Edit the name of Infrastructure Resource Group** 
- In Advanced tab:   
   - If you need to modify the name of infrastructure resource group of AKS, click on Edit and change the name.      

**6. Click Review + Create, validate the configuration, and then click Create to deploy the cluster.**   

## What looks like Infrastructure Resource Group Resources

The following resources are provisioned as part of the AKS cluster deployment. 

![Infrastructure Resource Group](images/Private-EndpointAKS.png)


## 🔗  Link Hub VNet (VNet where Cloud Shell ACI will be deployed) to Private DNS Zone. 
**1. Locate the AKS Private DNS Zone**
- Go to Private DNS Zones in Azure Portal.
- The zone name will look like: privatelink.<region>.azmk8s.io (e.g., privatelink.northeurope.azmk8s.io)
**2. Link the Hub VNet in my case to the Private DNS Zone**
- In the Private DNS Zone:
  - Go to Settings → Virtual Network Links
  - Click + Add
    
![Link Virtual Network to Private DNS Zone](images/Link-hub-vnet-to-private-dns-zone.png)

 
- Enter a Link Name (e.g., dnslink-vnet-hub)
- Select the Hub VNet from the dropdown
- Choose whether to enable Auto-registration (usually disabled for AKS)
- Click Create

![Link Virtual Network to Private DNS Zone](images/Private-EndpointAKS-2.png)


## 🔧 Prerequisites for Cloud Shell VNet Integration
To configure Azure Cloud Shell with Virtual Network integration, the following components must be provisioned:

- **Network Profile**: Defines the network configuration for the Cloud Shell container, including subnet and IP settings.
- **Azure Relay Namespace**: Facilitates secure communication between the Cloud Shell container and Customer Browser.
- **File Share (Storage Account)**: Used to persist Cloud Shell session data across restarts. 

**PS: The above resources are automatically deployed via the ARM Template Azure Cloud Shell,except the File Share that should be deployed manually.**

### <img width="68" height="56" alt="image" src="https://github.com/user-attachments/assets/732f855a-93de-4cfe-8167-2c4d42430b9a" /> Create a Storage Account in Azure Portal
**1. Go to Azure Portal** 
Navigate to https://portal.azure.com and sign in.

**2. Search for "Storage Accounts"**
In the top search bar, type Storage Accounts and select it.  

**3. Click "Create"**
Click the + Create button to start the wizard.  

**4. Fill in the Basics:**

- Subscription: Select your active subscription.
- Resource Group: A resource group where you will have Storage account and Azure Relay namespace.
- Storage Account Name: Enter a globally unique name (e.g., hubstorageaccount).
- Region: Select North Europe.
- Performance: Standard.
- Redundancy: Choose Locally-redundant storage (LRS). (Unless you need high availability but for the sake of this demo I used LRS) 
- Click "Review + Create", then Create
- Azure will validate the configuration and deploy the storage account.

![Create Storage accout](images/Create-Storage-Account-1.png)

#### https://i.imgur.com/FHhdfqG.png Create a File Share Inside the Storage Account
Navigate to the Storage Account
Once deployed, go to the newly created storage account.

Select "File shares" from the left menu
Under Data storage, click File shares.

Click "+ File share"
Provide:

Name: e.g., cloudshellfileshare
Quota: Optional (e.g., 5 GB)
Click "Create"
Your file share will be created and ready for use.

## <img width="50" height="50" alt="image" src="https://github.com/user-attachments/assets/e58663d2-1b30-4081-af94-cd28dec08937" />  Azure Relay Deployment & configure Cloud Shell

- Deploy Azure Relay
   
- Configure Cloud Shell within the Hub VNet (or VNet of your choice that is peered to AKS VNet)
- Use Cloud Shell to securely connect to the AKS private endpoint

  
## ✅ Connection Verification
Confirm access paths:
From Portal → Cloud Shell → Private Endpoint AKS


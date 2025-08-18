### Why You Can’t See the NIC or Private IP of the Cloud Shell Container  

Even though the Cloud Shell container runs inside your VNet, its network interface is not exposed to you. Here's why:  
 **1. Cloud Shell Is a Managed Service**   
    - The container is provisioned and controlled entirely by Microsoft.    
	- You don’t own the container group or its NIC—so it doesn’t show up in your subscription.  

 **2. No Resource Visibility**  
	- The NIC is created in the background, but it’s not listed under your az network nic list.    
	- You won’t see it in the Azure Portal, Resource Graph, or CLI.     
 **3. Ephemeral and Isolated**     
	- The container is short-lived and isolated.     


### 🔌Azure Relay Acts as a Bi-directional Proxy—but Only for the Browser ↔ Container Path  
**1. Cloud Shell Container Is Deployed Inside Your VNet**     
	- It gets a private IP from the container subnet.     
	- It can access private resources like AKS, VMs, etc.     
 **2. Azure Relay Is Used by Microsoft to Reach the Container**    
	- Azure Relay is not accessed by your resources.     
	- Instead, Microsoft’s infrastructure uses Azure Relay to connect your browser session to the container.         
	- This is done via a hybrid connection, which allows two endpoints that aren’t directly reachable to communicate securely.      
 **3. Private Endpoint for Azure Relay Is Deployed in Your VNet**    
	- This gives Azure Relay a private IP inside your VNet.      
	- Azure uses this private IP to reach the Cloud Shell container from its own infrastructure.     
	- The traffic flow is initiated by Azure Relay (on behalf of your browser), not by your resources.     

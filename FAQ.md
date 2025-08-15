### Why You Can’t See the NIC or Private IP of the Cloud Shell Container  
Even though the Cloud Shell container runs inside your VNet, its network interface is not exposed to you. Here's why:  
 1. Cloud Shell Is a Managed Service  
	- The container is provisioned and controlled entirely by Microsoft.  
	- You don’t own the container group or its NIC—so it doesn’t show up in your subscription.  
 2. No Resource Visibility  
	- The NIC is created in the background, but it’s not listed under your az network nic list.
	- You won’t see it in the Azure Portal, Resource Graph, or CLI.
 3. Ephemeral and Isolated
	- The container is short-lived and isolated.


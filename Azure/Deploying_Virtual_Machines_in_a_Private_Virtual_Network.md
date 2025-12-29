The Nautilus DevOps team is expanding their Azure infrastructure and requires the setup of a private Virtual Network (VNet) along with a subnet.  
This VNet and subnet configuration will ensure that resources deployed within them remain isolated from external networks and can only communicate within the VNet.  
Additionally, the team needs to provision a Virtual Machine (VM) under the newly created private VNet. This VM should be accessible over SSH from within the VNet only, allowing for secure communication and resource management within the Azure environment.  
The name of the VNet must be xfusion-priv-vnet, create a subnet named xfusion-priv-subnet under the same. Further, create a Virtual Machine named xfusion-priv-vm under this VNet.  
Additionally, create a Network Security Group (NSG) named xfusion-priv-nsg, and ensure that the NSG rules for the VM allow access only from within the VNet's CIDR block. Ensure all resources are created in the East US region.  

Solution:  
```bash
rg=$(az group list --query "[].name" -o tsv)
az vnet create -g $rg -n xfusion-priv-vnet --location eastus --subnet-name xfusion-priv-subnet
az network nsg create -g $rg -n xfusion-priv-nsg --location eastus
az network nsg rule create -g $rg --nsg-name xfusion-priv-nsg -n AllowVnetOnly --priority 1010 --access Allow --source-address-prefixes 10.0.0.0/16 --destination-adderss-prefixes 10.0.0.0/16 --source-port-ranges '*' --destination-port-ranges '*'
az vm create -g $rg -n xfusion-priv-vm --image Ubuntu2204 --storage-sku Standard_LRS --size Standard_B1s --admin-username azureuser --vnet-name xfusion-priv-vnet --subnet xfusion-priv-subnet --location eastus --nsg xfusion-priv-nsg
```

[reference](https://learn.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az-vm-create)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

The Nautilus DevOps team has been tasked with demonstrating the use of VNet Peering to enable communication between two VNets. One VNet will be a private VNet that contains a private Azure VM, while the other will be a public VNet containing a publicly accessible Azure VM.  
1) Existing Azure Resources:  
Public VM: devops-pub-vm is already in the public VNet.  
Private VNet and VM: devops-priv-vnet and devops-priv-vm exist in the private VNet with its subnet: devops-priv-subnet.  
2) Create VNet Peering:  
Create a VNet Peering between the Public VNet and Private VNet.  
VNet Peering Name: devops-pub-to-priv-peering.  
3) Test the Connection:  
SSH into the public VM and verify that you can ping the private VM

Solution:  
```
RG=$(az group lsit --query "[].name" -o tsv)
PUBLIC_VNET_ID=$(az network vnet show \
  --resource-group $RG \
  --name devops-pub-vnet \
  --query id --output tsv)
PRIVATE_VNET_ID=$(az network vnet show \
  --resource-group $RG \
  --name devops-priv-vnet \
  --query id --output tsv)
az network vnet peering create \
  --name devops-pub-to-priv-peering \
  --resource-group $RG \
  --vnet-name devops-pub-vnet \
  --remote-vnet $PRIVATE_VNET_ID \
  --allow-vnet-access
az network vnet peering list \
  --resource-group $RG \
  --vnet-name devops-pub-vnet
```

[reference](https://learn.microsoft.com/en-us/cli/azure/network/vnet/peering?view=azure-cli-latest)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

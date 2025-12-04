The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking,  
they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.  
This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.  
Create a Virtual Network (VNet) named datacenter-vnet in the East US region with any IPv4 CIDR block.  
Use below given Azure Credentials: (You can run the showcreds command on the azure-client host to retrieve these credentials)  

Solution:  
```bash
mygroup=$(az group list --query "[].name" -o tsv)
az network vnet create --name datacenter-vnet --resource-group $mygroup
```

[reference](https://learn.microsoft.com/en-us/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-create)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

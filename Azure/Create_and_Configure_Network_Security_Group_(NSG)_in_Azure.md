The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.  
To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.  
By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.  
For this task, create a network security group (NSG) with the following requirements:  
Name of the NSG should be nautilus-nsg.  
Add an inbound security rule named Allow-HTTP for HTTP service on port 80, with the source CIDR range of 0.0.0.0/0.  
Add another inbound security rule named Allow-SSH for SSH service on port 22, with the source CIDR range of 0.0.0.0/0.  

Solution:  
```bash
rg=$(az group list --query "[].name" -o tsv)
az network nsg create -g $rg -n nautilus-nsg
az network nsg rule create -g $rg --nsg-name nautilus-nsg -n Allow-HTTP --priority 100 --access Allow --direction Inbound --source-address-prefixes 0.0.0.0/0 --destination-port-ranges 80 --source-port-ranges 80 --protocol Tcp
az network nsg rule create -g $rg --nsg-name nautilus-nsg -n Allow-SSH --priority 110 --access Allow --direction Inbound --source-address-prefixes 0.0.0.0/0 --destination-port-ranges 22 --source-port-ranges 22 --protocol Tcp
```

[reference](https://learn.microsoft.com/en-us/cli/azure/network/nsg/rule?view=azure-cli-latest#az-network-nsg-rule-update)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

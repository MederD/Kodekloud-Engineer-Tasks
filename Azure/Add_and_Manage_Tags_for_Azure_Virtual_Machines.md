The Nautilus DevOps team is migrating a portion of their infrastructure to Azure. During the migration, they have created several virtual machines (VMs) in different regions.  
The team has identified one VM that is not tagged properly so they decided to tag it as needed.  
Add the tag Environment=dev to the virtual machine named nautilus-vm.   

Solution:  
```bash
mygroup=$(az group list --query "[].name" -o tsv)
resource=$(az resource show -g $mygroup -n nautilus-vm --resource-type Microsoft.Compute/virtualMachines --query "id" -o tsv
az tag create --resource-id $resource --tags Environment=dev
```

[reference](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-cli)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

As part of the data migration process, the Nautilus DevOps team is actively creating several storage containers on Azure.  
They plan to utilize private Blob containers to store the relevant data. Given the ongoing migration of other infrastructure to Azure, it is logical to consolidate data storage within the Azure environment as well.  
Create a new storage account named `xfusionst15788` and a private Blob container named `xfusion-blob-32051` within the storage account.  

Solution:  
```bash
rg=$(az group list --query "[].name" -o tsv)
az storage account create -n xfusionst15788 -g $rg
az storage container create -n xfusion-blob-32051 --account-name xfusionst15788 --auth-mode login
```

[reference](https://learn.microsoft.com/en-us/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

The Nautilus DevOps team is currently engaged in a cleanup process, focusing on removing unnecessary data and services from their Azure environment.  
As part of the migration process, several resources were created for one-time use only, necessitating a cleanup effort to optimize their Azure environment.  
A private blob container named devops-blob-27934 already exists in the eastus region under storage account devopsst9293.  
1) Copy the contents of devops-blob-27934 blob container to the /opt directory on the azure-client host (the landing host once you load this lab).  
2) Delete the blob container devops-blob-27934 from the storage account.

Solution:  
```
RG=$(az group list --query "[].name" -o tsv)
STORAGE_ACCOUNT_KEY=$(az storage account keys list -g $RG --account-name devopsst9293 --query "[0].value" -o tsv)
az storage blob download-batch \
  --account-name devopsst9293 \
  --source devops-blob-27934 \
  --destination /opt \
  --overwrite
ls -la /opt
az storage blob delete-batch --account-name devopsst9293 --source devops-blob-27934
az storage container delete --name devops-blob-27934 --account-name devopsst9293
```
[reference](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-install-linux-package?tabs=apt)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

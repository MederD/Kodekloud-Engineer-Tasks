The Nautilus DevOps team has been tasked with creating an internal information portal for public access. As part of this project, they need to host a static website on Azure using an Azure Storage account.  
The Storage account must be configured for public access to allow external users to access the static website directly via the Azure Storage URL.  
Task Requirements:  
Create an Azure Storage account named datacenterwebst24805 in an existing resource group.  
Configure the Storage account for static website hosting with index.html as the index document.  
Allow public access to the static website so that the website is publicly accessible.  
Upload the index.html file from the /root/ directory of the Azure client host to the Storage account's $web container.  
Verify that the website is accessible directly through the Azure Storage static website URL.  

Solution:  
```
RG=$(az group list --query "[].name" -o tsv)
az storage account create --name datacenterwebst24805 --resource-group $RG --sku Standard_LRS --kind StorageV2
az storage blob service-properties update --account-name datacenterwebst24805 --static-website --index-document index.html
az storage blob upload --account-name datacenterwebst24805 --container-name \$web --name index.html --file /root/index.html --auth-mode login
az storage account show --name datacenterwebst24805 --resource-group $RG --query "primaryEndpoints.web" -o tsv
```

[reference](https://learn.microsoft.com/en-us/azure/storage/blobs/blob-containers-cli)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)


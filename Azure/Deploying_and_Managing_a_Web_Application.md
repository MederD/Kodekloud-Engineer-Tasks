The Nautilus DevOps team is tasked with deploying a Python-based web application on Azure. You need to create a web app using the following specifications:  
1) The Web App name should be datacenter-webapp.  
2) It should be created in the West US region under the default resource group.  
3) The publish option should be set to Code.  
4) The Runtime Stack should be Python with Linux as the operating system.  
5) Create a new App Service Plan named datacenter-learn-python with the SKU Basic B1.  
6) Application Insights should be disabled.  
7) Add tags:  
Name: WebAppLearning  
Environment: Dev  
Make sure the web app is in Running state after creation.

Solution:  
```
rg=$(az group list --query "[].name" -o tsv)
az appservice plan create \
  --name datacenter-learn-python \
  --resource-group "$rg" \
  --location "West US" \
  --sku B1 \
  --is-linux
az webapp create \
  --name datacenter-webapp \
  --resource-group "$rg" \
  --plan datacenter-learn-python \
  --runtime "PYTHON:3.10"\
  --tags Name=WebAppLearning Environment=Dev
az webapp show \
  --name datacenter-webapp \
  --resource-group "$rg" \
  --query "state" -o tsv

```
[reference](https://learn.microsoft.com/en-us/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

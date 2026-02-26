The Nautilus DevOps team needs to set up an Azure Virtual Machine (VM) to interact with an Azure Blob Storage container for storing and retrieving data.  
The team must create a private storage account, configure Blob Storage, and test the functionality.  
Task:  
1) Azure Virtual Machine Setup:  
The VM named datacenter-vm already exists in the East US region.  
2) Create a Private Storage Account and Blob Container:  
Create a storage account named datacenterstor1954 in the East US region with Locally-redundant storage (LRS).  
Create a private Blob container named datacenter-container1954.  
3) Retrieve Storage Account Key:  
Get the storage account's access key to configure access for the application.  
4) Create a Test File:  
SSH into the VM and create a file named testfile.txt in the /home/azureuser directory with content: "this is a test file".  
5) Upload the File to Blob Storage:  
Upload the testfile.txt file to the Blob container datacenter-container1954 using the Azure CLI command:  
`az storage blob upload --account-name datacenterstor1954 --account-key <access-key> --container-name datacenter-container1954 --name testfile.txt --file /home/azureuser/testfile.txt`

Solution:  
```
RG=$(az group list --query "[].name" -o tsv | head -n 1)
PUBLIC_IP=$(az vm show -d -g $RG -n datacenter-vm --query publicIps -o tsv)

az storage account create \
  --name datacenterstor1954 \
  --resource-group $RG \
  --location "East US" \
  --sku Standard_LRS \
  --kind StorageV2 \
  --allow-blob-public-access false

az storage container create \
  --name datacenter-container1954 \
  --account-name datacenterstor1954 \
  --auth-mode login

ACCESS_KEY=$(az storage account keys list \
  --account-name datacenterstor1954 \
  --resource-group $RG \
  --query "[0].value" -o tsv)

ssh azureuser@$PUBLIC_IP << EOF
  echo "this is a test file" > /home/azureuser/testfile.txt
  
  az storage blob upload \
    --account-name datacenterstor1954 \
    --account-key '$ACCESS_KEY' \
    --container-name datacenter-container1954 \
    --name testfile.txt \
    --file /home/azureuser/testfile.txt \
    --auth-mode key
     
  az storage blob list \
    --account-name datacenterstor1954 \
    --account-key '$ACCESS_KEY' \
    --container-name datacenter-container1954 \
    --output table
EOF

```
[reference](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-cli)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

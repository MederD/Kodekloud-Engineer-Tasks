The Nautilus DevOps team needs to optimize data retention costs by automating the deletion of old blobs. They plan to implement Blob Lifecycle Management for a specific container in Azure Storage.  
Task:  
1) Create a Storage Account:  
Name the storage account datacenterstor20023.  
Set the region to East US.  
Use Locally-redundant storage (LRS) as the redundancy option.  
2) Create a Blob Container:  
Name the container datacenter-container20023.  
3) Upload a File to the Container:  
Upload the file named tempfile.txt to the container. The file is present under /root of the client host.  
4) Configure Blob Lifecycle Management:  
Apply a Lifecycle Management rule named datacenter-del-rule to the container datacenter-container20023 to delete blobs after 7 days of last modification.  
5) Validation:  
Verify that the Lifecycle Management rule named datacenter-del-rule is correctly applied.

Solution:  
```
RG=$(az group list --query "[].name" -o tsv)

az storage account create \
  --name datacenterstor20023 \
  --resource-group "$RG" \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2

CONN_STR=$(az storage account show-connection-string \
  --name datacenterstor20023 \
  --resource-group "$RG" \
  --query connectionString -o tsv)

az storage container create \
  --name datacenter-container20023 \
  --connection-string "$CONN_STR"

az storage blob upload \
  --container-name datacenter-container20023 \
  --file /root/tempfile.txt \
  --name tempfile.txt \
  --connection-string "$CONN_STR"

cat > datacenter-del-rule.json << 'EOF'
{
  "rules": [
    {
      "enabled": true,
      "name": "datacenter-del-rule",
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "datacenter-container20023/" ]
        },
        "actions": {
          "baseBlob": {
            "delete": { "daysAfterModificationGreaterThan": 7 }
          }
        }
      }
    }
  ]
}
EOF

az storage account management-policy create \
  --account-name datacenterstor20023 \
  --resource-group "$RG" \
  --policy @datacenter-del-rule.json

az storage account management-policy show \
  --account-name datacenterstor20023 \
  --resource-group "$RG" \
  --query "policy.rules[?name=='datacenter-del-rule']"
```

[reference](https://learn.microsoft.com/en-us/cli/azure/storage?view=azure-cli-latest)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

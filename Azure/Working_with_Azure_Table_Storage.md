The Nautilus DevOps team is developing a simple 'To-Do' application using Azure Table Storage to store and manage tasks efficiently.  
The team needs to create an Azure Table to hold tasks, each identified by a unique taskId.  
Each task will have a description and a status, which indicates the progress of the task (e.g., 'completed' or 'in-progress').  
Your task is to:  
&nbsp;&nbsp;&nbsp;Create an Azure Storage Account named devopstablest18462 with a Table Storage table called tasks.  
&nbsp;&nbsp;&nbsp;Insert the following tasks into the table:  
&nbsp;&nbsp;&nbsp;Task 1: PartitionKey: 'tasks', RowKey: '1', description: 'Learn Table Storage', status: 'completed'  
&nbsp;&nbsp;&nbsp;Task 2: PartitionKey: 'tasks', RowKey: '2', description: 'Build To-Do App', status: 'in-progress'  
&nbsp;&nbsp;&nbsp;Verify that Task 1 has a status of 'completed' and Task 2 has a status of 'in-progress'.  
&nbsp;&nbsp;&nbsp;Note: Use the Azure CLI to insert these tasks into the table.  

Solution:  
```
RG=$(az group list --query "[].name" -o tsv)
STORAGE_ACCOUNT="devopstablest18462"
-
az storage account create \
  --name $STORAGE_ACCOUNT \
  --resource-group $RG \
  --sku Standard_LRS \
  --kind StorageV2
-
az storage table create \
  --name tasks \
  --account-name $STORAGE_ACCOUNT \
  --auth-mode login
-
az storage entity insert  \
  --table-name tasks  \
  --account-name $STORAGE_ACCOUNT  \
  --auth-mode login  \
  --entity PartitionKey='tasks' RowKey='1' description='Learn Table Storage' status='completed'
-
az storage entity insert  \
  --table-name tasks  \
  --account-name $STORAGE_ACCOUNT  \
  --auth-mode login  \
  --entity PartitionKey='tasks' RowKey='2' description='Build To-Do App' status='in-progress'
-
az storage entity show \
  --table-name tasks \
  --partition-key tasks \
  --row-key 1 \
  --account-name $STORAGE_ACCOUNT \
  --auth-mode login \
  --select status
```
[reference](https://learn.microsoft.com/en-us/cli/azure/storage/entity?view=azure-cli-latest#az-storage-entity-insert)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

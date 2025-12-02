The Nautilus DevOps team is planning to migrate a portion of their infrastructure to the Azure cloud incrementally. As part of this migration, you are tasked with creating an Azure Virtual Machine (VM).  
The requirements are:  
1) Use the existing resource group.  
2) The VM name must be datacenter-vm, it should be in West US region.  
3) Use the Ubuntu 22.04 LTS image for the VM.  
4) The VM size must be Standard_B1s.  
5) Attach a default Network Security Group (NSG) that allows inbound SSH (port 22).  
6) Attach a 30 GB storage disk of type Standard HDD.  
7) The rest of the configurations should remain as default.  
After completing these steps, make sure you can SSH into the virtual machine.  
Use below given Azure Credentials: (You can run the showcreds command on the azure-client host to retrieve credentials)

Solution:  
```bash
az vm create \
  --resource-group kml_rg_main-eb46db24f2ad4634 \
  --name datacenter-vm \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --nsg-rule SSH \
  --storage-sku Standard_LRS \
  --os-disk-size-gb 30 \
  --admin-username kk_lab_user \
  --generate-ssh-keys
```

[reference](https://learn.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks

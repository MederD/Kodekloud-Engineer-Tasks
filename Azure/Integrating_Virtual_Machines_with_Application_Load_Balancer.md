The Nautilus DevOps team is currently working on setting up a simple application on the Azure cloud. They aim to establish an Azure Load Balancer in front of a Virtual Machine (VM) where an Nginx server is currently running.  
While the Nginx server currently serves a sample page, the team plans to deploy the actual application later.  
Set up an Azure Load Balancer named devops-lb.  
Configure the Load Balancer’s frontend IP configuration with the name devops-lb-ip and assign a public IP address with the same name (devops-lb-ip).  
Create a backend pool named devops-backend-pool and add the VM running Nginx to this pool.  
Create a health probe named devops-health-probe on port 80 to check the VM's health.  
Set up a load balancer rule named devops-lb-rule to route traffic on port 80 to the backend pool on port 80.  
Add an inbound rule to the existing NSG of the VM to allow HTTP traffic on port 80.  

Solution:  
```bash
az vm list -o table

VM_NAME=<VM_NAME>
RG=$(az group lsit --query "[].name" -o tsv)

az network nic ip-config list \
  --resource-group $RG \
  --nic-name $NIC_NAME \
  -o table
IPCONFIG_NAME=<FROM_OUTPUT>

NIC_ID=$(az vm show -g $RG -n $VM_NAME --query "networkProfile.networkInterfaces[0].id" -o tsv)
NIC_NAME=$(basename $NIC_ID)
NSG_NAME=$(az network nic show -g $RG -n $NIC_NAME --query "networkSecurityGroup.id" -o tsv | awk -F'/' '{print $NF}')

az network nsg rule create \
  --resource-group $RG \
  --nsg-name $NSG_NAME \
  --name allow-http-80 \
  --priority 1001 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --source-address-prefix '*' \
  --source-port-range '*' \
  --destination-address-prefix '*' \
  --destination-port-range 80

az network public-ip create \
  --resource-group $RG \
  --name devops-lb-ip \
  --sku Standard \
  --allocation-method Static

az network lb create \
  --resource-group $RG \
  --name devops-lb \
  --sku Standard \
  --frontend-ip-name devops-lb-ip \
  --public-ip-address devops-lb-ip \
  --backend-pool-name devops-backend-pool

az network nic ip-config address-pool add \
  --address-pool devops-backend-pool \
  --ip-config-name $IPCONFIG_NAME \
  --nic-name $NIC_NAME \
  --resource-group $RG \
  --lb-name devops-lb

az network lb probe create \
  --resource-group $RG \
  --lb-name devops-lb \
  --name devops-health-probe \
  --protocol Tcp \
  --port 80

az network lb rule create \
  --resource-group $RG \
  --lb-name devops-lb \
  --name devops-lb-rule \
  --protocol Tcp \
  --frontend-port 80 \
  --backend-port 80 \
  --frontend-ip-name devops-lb-ip \
  --backend-pool-name devops-backend-pool \
  --probe-name devops-health-probe

IP=$(az network public-ip show -g $RG -n devops-lb-ip --query ipAddress -o tsv)
curl -v http://$IP
```

[reference](https://learn.microsoft.com/en-us/cli/azure/network/lb?view=azure-cli-latest)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

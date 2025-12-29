The Nautilus DevOps Team deployed an Nginx server on an Azure VM in a public VNet named datacenter-vnet. However, the server is still inaccessible from the internet.  
As a DevOps team member, complete the following tasks:  
    Verify VNet Configuration: Ensure datacenter-vnet allows internet access.  
    Attach Public IP: A public IP named datacenter-pip already exists. Attach this public IP to the VM datacenter-vm to make it accessible from the internet.  
    Ensure Accessibility: Confirm the VM datacenter-vm is accessible on port 80.  

Solution:  
```bash
nic_id=$(az vm show -d -g $rg -n datacenter-vm --query "networkProfile.networkInterfaces[].id" -o tsv)
nic_name=$(az network nic show -g $rg --ids $nic_id --query "name" -o tsv)
config_name=$(az network nic show -g $rg -n $nic_name --query "ipConfigurations[].name" -o tsv)
az network nic ip-config update --name $config_name --nic-name $nic_name -g $rg --public-ip-address datacenter-pip
az vm open-port -g $rg -n datacenter-vm --port 80
rtb_name=$(az network route-table list -g $rg --query "[].name" -o tsv)
az network route-table route update -g $rg --route-table-name $rtb_name --name Block-Internet --address-prefix 0.0.0.0/0 --next-hop-type Internet
pip=$(az network public-ip show -g $rg -n datacenter-pip --query "ipAddress" -o tsv)
ssh azureuser@$pip
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
curl -I http://$pip:80
```

[reference](https://learn.microsoft.com/en-us/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-update)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

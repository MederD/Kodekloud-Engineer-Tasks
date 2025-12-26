The Nautilus DevOps team needs to expand the storage capacity of an existing virtual machine and add an additional data disk to support increased workloads.  
This task requires resizing the existing VM disk and mounting a new data disk to the VM.  
As a member of the team, perform the following steps:  
1) Expand the existing VM xfusion-vm disk from 32Gi to 64Gi.  
2) Also create a new standard HDD data disk named xfusion-disk of 64Gi and mount the disk to VM xfusion-vm at location /mnt/xfusion-disk.

Solution:  
```bash
rg=$(az group list --query "[].name" -o tsv)
osd=$(az vm show -d -g $rg -n xfusion-vm --query "storageProfile.osDisk.name" -o tsv)
az vm stop -g $rg -n xfusion-vm
az vm deallocate -g $rg -n xfusion-vm
az disk update -g $rg --name $osd --size-gb 64

az vm disk attach \
  --resource-group $rg \
  --vm-name xfusion-vm \
  --name xfusion-disk \
  --new \
  --size-gb 64 \
  --sku Standard_LRS

az vm user update \
  --resource-group $rg \
  --name xfusion-vm \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub

az vm start -g $rg -n xfusion-vm
pip=$(az vm show -d -g $rg -n xfusion-vm --query "publicIps[0]" -o tsv)
ssh azureuser@$pip

--- INSIDE VM
sudo lsblk
sudo parted /dev/sdc mklabel gpt mkpart primary ext4 0% 100%
sudo mkfs.ext4 /dev/sdc1
sudo mkdir -p /mnt/xfusion-disk
sudo mount /dev/sdc1 /mnt/xfusion-disk
uid=$(sudo blkid -s UUID -o value /dev/sdc1)
echo "UUID=$uid /mnt/xfusion-disk ext4 defaults,nofail 0 2" | sudo tee -a /etc/fstab
```

[reference](https://learn.microsoft.com/en-us/cli/azure/vm/disk?view=azure-cli-latest#az-vm-disk-attach)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

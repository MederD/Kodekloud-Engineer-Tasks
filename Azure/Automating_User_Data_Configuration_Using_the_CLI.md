The Nautilus DevOps Team is working on setting up a new virtual machine (VM) to host a web server for a critical application.  
The team lead has requested you to create an Azure VM that will serve as a web server using Nginx. This VM will be part of the initial infrastructure setup for the Nautilus project.  
Ensuring that the server is correctly configured and accessible from the internet is crucial for the upcoming deployment phase.  
As a member of the Nautilus DevOps Team, your task is to create a VM using Azure CLI with the following specifications:  
Instance Name: The VM must be named devops-vm.  
Image: Use any available Ubuntu image to create this VM.  
Custom Script Extension/User Data: Configure the VM to run a custom script during its launch. This script should:  
    Install the Nginx package.  
    Start the Nginx service.  
Network Security Group (NSG): Ensure that the VM allows HTTP traffic on port 80 from the internet.  
Instructions:  
    Use Azure CLI commands to set up the VM in the specified configuration.  
    Ensure the VM is accessible from the internet on port 80.  
    The Nginx service should be running after setup.  
Use the Azure CLI commands to complete the task.   

Solution:  
```bash
rg=$(az group list --query "[].name" -o tsv)

az vm create \
   -g $rg \
   -n devops-vm \
   --image Ubuntu2204 \
   --storage-sku Standard_LRS \
   --custom-data data.txt \
   --size Standard_B1s \
   --admin-username azureuser \
   --generate-ssh-keys

az vm open-port --port 80 -g $rg -n devops-vm
```
`data.txt:`  
```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
runcmd:
  - systemctl start nginx
  - systemctl enable nginx
```
[reference](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-automate-vm-deployment)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

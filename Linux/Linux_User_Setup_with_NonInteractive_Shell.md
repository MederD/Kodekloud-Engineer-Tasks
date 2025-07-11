To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell. Here's your task:  
Create a user named james with a non-interactive shell on App Server 1.  

Solution:  
```
ssh tony@stapp01
sudo useradd james 0s /sbin/nologin
```

[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip)    
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  


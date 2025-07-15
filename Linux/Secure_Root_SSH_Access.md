Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.  
Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.  

Solution:  
```
ssh tony@stapp01
sudo vi /etc/ssh/sshd_config
```
find `PermitRootLogin yes` replace to ` PermitRootLogin no`  
```
sudo systemctl restart sshd
```
[reference](https://www.ionos.com/help/server-cloud-infrastructure/getting-started/important-security-information-for-your-server/deactivating-the-ssh-root-login/)    
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main) 


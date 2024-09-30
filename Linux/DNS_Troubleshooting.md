The system admins team of xFusionCorp Industries has noticed intermittent issues with DNS resolution in several apps.  
App Server 1 in Stratos Datacenter is having some DNS resolution issues, so we want to add some additional DNS nameservers on this server.  
As a temporary fix we have decided to go with Google public DNS (ipv4). Please make appropriate changes on this server.  

Solution:  
```bash
ssh tony@stapp01
sudo vi /etc/resolv.conf
```
add below:  
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

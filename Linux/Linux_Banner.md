During the monthly compliance meeting, it was pointed out that several servers in the Stratos DC do not have a valid banner. 
The security team has provided serveral approved templates which should be applied to the servers to maintain compliance. These will be displayed to the user upon a successful login.  
Update the message of the day on all application and db servers for Nautilus. Make use of the approved template located at /home/thor/nautilus_banner on jump host  

Solution:
1 - Copy banner file to all servers:
```
sudo scp /home/thor/nautilus_banner peter@stdb01:/home/peter
```
2 - SSH to the server and edit the config file:
```
sudo vi /etc/ssh/sshd_config
```
3 - Uncomment the "Banner" and add the path
```
---
Banner /home/peter/nautilus_banner
```
4 - Restart sshd"
```
sudo systemctl restart sshd
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

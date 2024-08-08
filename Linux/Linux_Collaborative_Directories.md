The Nautilus team doesn't want its data to be accessed by any of the other groups/teams due to security reasons and want their data to be strictly accessed by the sysadmin group of the team.  
Setup a collaborative directory /sysadmin/data on app server 1 in Stratos Datacenter.  
The directory should be group owned by the group sysadmin and the group should own the files inside the directory.   
The directory should be read/write/execute to the user and group owners, and others should not have any access.  

Solution:
```bash
ssh tony@stapp01
sudo su
mkdir -p /sysadmin/data
chgrp -R sysadmin /sysadmin/data
chmod -R 2770 /sysadmin/data
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

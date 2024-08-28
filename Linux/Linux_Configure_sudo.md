We have some users on all app servers in Stratos Datacenter. Some of them have been assigned some new roles and responsibilities, therefore their users need to be upgraded with sudo access so that they can perform admin level tasks.  

a. Provide sudo access to user yousuf on all app servers.  

b. Make sure you have set up password-less sudo for the user.  

Solution:  
```bash
ssh tony@stapp01
sudo su
usermod -aG wheel yousuf
vi /etc/sudoers ->>>> uncomment %wheel ALL=(ALL) ALL NOPASSWD(ALL)
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 


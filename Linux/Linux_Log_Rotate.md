The Nautilus DevOps team is ready to launch a new application, which they will deploy on app servers in Stratos Datacenter.  
They are expecting significant traffic/usage of squid on app servers after that. This will generate massive logs, creating huge log files.  
To utilise the storage efficiently, they need to compress the log files and need to rotate old logs. Check the requirements shared below:  

a. In all app servers install squid package.  
b. Using logrotate configure squid logs rotation to monthly and keep only 3 rotated logs.  
(If by default log rotation is set, then please update configuration as needed)  

Solution:  
```
ssh tony@stapp01
sudo su
yum install squid -y
vi /etc/logrotate.d/squid
```
and then modify the file according to task.  

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  


The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:  
a. Install cronie package on all Nautilus app servers and start crond service.  
b. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.  

Solution:
```
- SSH to all app servers
- Change user to root
- yum update -y && yum install cronie -y
- check crond status
- vi /etc/crontab and copy the "*/5 * * * * echo hello > /tmp/cron_text" to the file
- systemctl restart crond
```
[reference](https://www.uptimia.com/questions/how-to-install-crontab-on-centos-rhel-linux)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

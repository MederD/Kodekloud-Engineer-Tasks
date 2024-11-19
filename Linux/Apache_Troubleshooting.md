xFusionCorp Industries uses some monitoring tools to check the status of every service, application, etc running on the systems.  
Recently, the monitoring system identified that Apache service is not running on some of the Nautilus Application Servers in Stratos Datacenter.  
1. Identify the faulty Nautilus Application Server and fix the issue. Also, make sure Apache service is up and running on all Nautilus Application Servers. Do not try to stop any kind of firewall that is already running.  
2. Apache is running on 5004 port on all Nautilus Application Servers and its document root must be /var/www/html on all app servers.  
3. Finally you can test from jump host using curl command to access Apache on all app servers and it should be reachable and you should get some static page. E.g. curl http://172.16.238.10:5004/.

Solution: 
```
ssh tony@stapp01
sudo systemctl start httpd
journalctl -xeu httpd.service
```
The last command will show configuration errors and the line number.  
```
sudo vi /etc/httpd/conf/httpd.conf
```
Fix the syntax errors, which can be different on each server. Ex: DocumentRoot "var/www/html" needs to be cahnged to DocumentRoot var/www/html.

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems.  
One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.  
Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts.  
They might not hosted any code yet on these servers so you need not to worry about if Apache isn't serving any pages or not, just make sure service is up and running. Also, make sure Apache is running on port 5002 on all app servers.  

Solution:  
```
curl stapp01:5002
curl stapp02:5002
curl stapp03:5002
```
Identify which server is not responing, in my case it was `stapp01`, then:  
```
ssh tony@stapp01
sudo systemctl status httpd
sudo systemctl start httpd
sudo journalctl -xe -u httpd -> This will tell exactly where the issue is
sudo netstat -tulpn
sudo ss -tuln | grep ':5002'
sudo kill <PROCESS_ID>
sudo systemctl start httpd
sudo systemctl status httpd
sudo systemctl enable httpd
exit
```
From jump_host check:  
```
curl stapp01:5002
```
[reference](https://www.redhat.com/en/blog/linux-command-basics-7-commands-process-management)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

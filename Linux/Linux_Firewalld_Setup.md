To secure our Nautilus infrastructure in Stratos Datacenter we have decided to install and configure firewalld on all app servers.  
We have Apache and Nginx services running on these apps. Nginx is running as a reverse proxy server for Apache.  
We might have more robust firewall settings in the future, but for now we have decided to go with the given requirements listed below:  

a. Allow all incoming connections on Nginx port, i.e 80.  
b. Block all incoming connections on Apache port, i.e 8080.  
c. All rules must be permanent.  
d. Zone should be public.  
e. If Apache or Nginx services aren't running already, please make sure to start them.  

Solution:  
```bash
ssh tony@stapp01
sudo yum install firewalld -y
sudo systemctl enable firewalld
sudo systemctl start firewalld
sudo firewall-cmd --state
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --zone=public --remove-port=8080/tcp --permanent
sudo firewall-cmd --reload
sudo systemctl status nginx
sudo systemctl status httpd
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

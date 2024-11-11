We are working on hardening Apache web server on all app servers. As a part of this process we want to add some of the Apache response headers for security purpose.  
We are testing the settings one by one on all app servers. As per details mentioned below enable these headers for Apache:  
```
Install httpd package on App Server 1 using yum and configure it to run on 5000 port, make sure to start its service.
Create an index.html file under Apache's default document root i.e /var/www/html and add below given content in it.
Welcome to the xFusionCorp Industries!
Configure Apache to enable below mentioned headers:
X-XSS-Protection header with value 1; mode=block
X-Frame-Options header with value SAMEORIGIN
X-Content-Type-Options header with value nosniff
```
Note: You can test using curl on the given app server as LBR URL will not work for this task.  

Solution:  
```
ssh tony@stapp01
sudo yum intall httpd -y
sudo sed -i 's/^Listen 80/Listen 5000/' /etc/httpd/conf/httpd.conf
echo "Welcome to the xFusionCorp Industries!" > /var/www/html/index.html
sudo chown apache.apache /var/www/html/index.html
sudo vi /etc/httpd/conf/httpd.conf
```
add below to configuration file:  
```
<IfModule mod_headers.c>
    Header set X-XSS-Protection "1; mode=block"
    Header set X-Frame-Options "SAMEORIGIN"
    Header set X-Content-Type-Options "nosniff"
</IfModule>
```
```
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```
test headers:  
```
curl -I http://localhost:5000
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

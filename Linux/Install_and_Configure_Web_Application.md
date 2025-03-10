xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:  
a. Install httpd package and dependencies on app server 2.  
b. Apache should serve on port 5000.  
c. There are two website's backups /home/thor/official and /home/thor/cluster on jump_host. Set them up on Apache in a way that official should work on the link http://localhost:5000/official/ and cluster should work on link http://localhost:5000/cluster/ on the mentioned app server.  
d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:5000/official/ and curl http://localhost:5000/cluster/  

Solution:  
```
scp /home/thor/official/index.html steve@stapp02:/tmp/official.html
scp /home/thor/cluster/index.html steve@stapp02:/tmp/cluster.html
ssh steve@stapp02
sudo yum install httpd -y
sudo mkdir -p /var/www/html/official
sudo mkdir -p /var/www/html/cluster
sudo mv /tmp/official.html /var/www/html/official/index.html
sudo mv /tmp/cluster.html /var/www/html/cluster/index.html
sudo vi /etc/httpd/conf/httpd.conf
```
Change the default port to 5000 by modifying the Listen directive.  
```
sudo vi /etc/httpd/conf.d/virtual_hosts.conf
```
Add below text:  
```
<VirtualHost *:5000>
    DocumentRoot /var/www/html

    <Directory "/var/www/html/official">
        AllowOverride All
        Require all granted
    </Directory>

    <Directory "/var/www/html/cluster">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```
```
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
curl http://localhost:5000/cluster/
exit
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

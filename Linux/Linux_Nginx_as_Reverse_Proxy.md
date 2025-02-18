Nautilus system admin's team is planning to deploy a front end application for their backup utility on Nautilus Backup Server, so that they can manage the backups of different websites from a graphical user interface.  
They have shared requirements to set up the same; please accomplish the tasks as per detail given below:  
a. Install Apache Server on Nautilus Backup Server and configure it to use 6300 port (do not bind it to 127.0.0.1 only, keep it default i.e let Apache listen on server's IP, hostname, localhost, 127.0.0.1 etc).  
b. Install Nginx webserver on Nautilus Backup Server and configure it to use 8098.  
c. Configure Nginx as a reverse proxy server for Apache.  
d. There is a sample index file /home/thor/index.html on Jump Host, copy that file to Apache's document root.  
e. Make sure to start Apache and Nginx services.  
f. You can test final changes using curl command, e.g curl http://<backup server IP or Hostname>:8098.  

Solution:
```bash
ssh clint@stbkp01
sudo yum install httpd -y
sudo vi /etc/httpd/httpd.conf
```
change this part:  
```
Listen 6300
```
```
sudo systemctl restart httpd
sudo systmctl status httpd
sudo systemctl enable httpd

sudo yum install nginx -y
sudo vi /etc/nginx/nginx.conf
```
change this part: 
```
server {
    listen 8098;
    server_name _;
    
    location / {
        proxy_pass http://localhost:6300;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
```
sudo nginx -t
sudo systemctl restart nginx
sudo systmctl status nginx
sudo systemctl enable nginx
```
Exit to jump_host:  
```
scp /home/thor/index.html clint@stbkp01:/tmp/
ssh clint@stbkp01
sudo mv /tmp/index.html /var/www/html
curl http://localhost:8098
exit
curl stbkp01:8098
```
curl should return:  
```
Welcome to xFusionCorp Industries!
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  


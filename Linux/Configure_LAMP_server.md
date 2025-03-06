xFusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter.  
They have already done infrastructure configuration—for example, on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory.  
Please perform the following steps to accomplish the task:  
a. Install httpd, php and its dependencies on all app hosts.  
b. Apache should serve on port 5003 within the apps.  
c. Install/Configure MariaDB server on DB Server.  
d. Create a database named `kodekloud_db10` and create a database user named `kodekloud_tim` identified as password `dCV3szSGNA`. Further make sure this newly created user is able to perform all operation on the database you created.  
e. Finally you should be able to access the website on LBR link, by clicking on the App button on the top bar. You should see a message like `App is able to connect to the database using user kodekloud_tim`.  

Solution:  
Create database and user:  
```
ssh peter@stdb01
sudo yum install mariadb-server mariadb -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo mysql -u root
MariaDB> create database kodekloud_db10;
MariaDB> grant all privileges on kodekloud_db10.* TO 'kodekloud_tim'@'%' identified by 'dCV3szSGNA';
MariaDB> flush privileges;
```
```
ssh tony@stapp01
sudo yum install httpd -y
sudo yum install yum install php php-mysqlnd php-pdo php-gd php-mbstring -y
```
Change port of Apache in /etc/httpd/conf/http.conf
```
sudo systemctl start httpd
sudo systemctl enable httpd
curl http://localhost:5003
```
[reference](https://www.ionos.com/help/server-cloud-infrastructure/server-administration/installing-a-lamp-stack-on-a-server-with-centos-stream-9/)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 





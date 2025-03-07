We need to setup a database server on Nautilus DB Server in Stratos Datacenter. Please perform the below given steps on DB Server:  
a. Install/Configure MariaDB server.  
b. Create a database named kodekloud_db8.   
c. Create a user called kodekloud_pop and set its password to TmPcZjtRQx.  
d. Grant full permissions to user kodekloud_pop on database kodekloud_db8  

Solution:  
```
ssh peter@stdb01
sudo yum install mariadb-server mariadb -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo mysql -u root
MariaDB> create database kodekloud_db8;
MariaDB> grant all privileges on kodekloud_db8.* TO 'kodekloud_pop'@'%' identified by 'TmPcZjtRQx';
MariaDB> flush privileges;
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

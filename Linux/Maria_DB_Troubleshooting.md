There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database.  
After digging into the issue, the team found that mariadb service is down on the database server.  
Look into the issue and fix the same.  

Solution:  
```
ssh peter@stdb01
sudo systemctl start mariadb
```
Above "start" command will result in error.  
```
sudo cat /var/log/mariadb/mariadb.log
```
And you will see the error logs:  
```
4-10-28 16:08:12 0 [ERROR] InnoDB: The error means mysqld does not have the access rights to the directory.
2024-10-28 16:08:12 0 [ERROR] InnoDB: Operating system error number 13 in a file operation.
2024-10-28 16:08:12 0 [ERROR] InnoDB: The error means mysqld does not have the access rights to the directory.
```
which suggests that there's permission errors:  
```
sudo chown mysql:mysql /var/lib/mysql
sudo chown mysql:mysql /var/run/mariadb
sudo start systemctl mariadb
```
[reference](https://dev.mysql.com/doc/mysql-secure-deployment-guide/5.7/en/secure-deployment-permissions.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

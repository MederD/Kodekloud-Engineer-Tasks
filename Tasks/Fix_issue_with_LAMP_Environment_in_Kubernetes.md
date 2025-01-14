One of the DevOps team member was trying to install a WordPress website on a LAMP stack which is essentially deployed on Kubernetes cluster.  
It was working well and we could see the installation page a few hours ago. However something is messed up with the stack now due to a website went down. Please look into the issue and fix it:  
FYI, deployment name is `lamp-wp` and its using a service named `lamp-service`. The Apache is using `http` default port and nodeport is `30008`.  
From the application logs it has been identified that application is facing some issues while connecting to the database in addition to other issues.  
Additionally, there are some environment variables associated with the pods like `MYSQL_ROOT_PASSWORD, MYSQL_DATABASE,  MYSQL_USER, MYSQL_PASSWORD, MYSQL_HOST`.  
Also do not try to delete/modify any other existing components like deployment name, service name, types, labels etc.  
Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  
```
k get svc
```
you will notice that service `lamp-service` has a port 8080. You will have to edit that yo port 80.  
Then:  
```
k logs lamp-wp-56c7c454fc-z95vp -c httpd-php-container
```
You will see the errors taht look like:  
```
[Tue Jan 14 17:43:11.784960 2025] [proxy_fcgi:error] [pid 121:tid 140711908309680] [client 10.244.0.1:22924]
AH01071: Got error 'PHP message: PHP Notice:  Undefined index: MYSQL_PASSWORDS in /app/index.php on line 4\nPHP message:
PHP Notice:  Undefined index: HOST_MYQSL in /app/index.php on line 5\nPHP message: PHP Warning:  mysqli_connect(): (HY000/2005):
Unknown MySQL server host 'mysql' (-2) in /app/index.php on line 8\n', referer: https://xpsynpij4z656ogo.labs.kodekloud.com/
```
This means there are syntax errors in /app/index.php.
Exec into the `http-php-container` and edit the /app/index.php file.

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

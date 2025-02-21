We have a requirement where we want to password protect a directory in the Apache web server document root.  
We want to password protect http://<website-url>:<apache_port>/protected/ URL as per the following requirements (you can use any website-url for it like localhost since there are no such specific requirements as of now).  
Setup the same on App server 3 as per below mentioned requirements:  
a. We want to use basic authentication.  
b. We do not want to use htpasswd file based authentication. Instead, we want to use PAM authentication, i.e Basic Auth + PAM so that we can authenticate with a Linux user.  
c. We already have a user javed with password TmPcZjtRQx which you need to provide access to.  
d. You can test the same using a curl command from jump host curl http://<website-url>:<apache_port>/protected/.  

Solution:
```
ssh banner@stapp03
sudo su
dnf -y install mod_authnz_pam
vi /etc/httpd/conf.modules.d/55-authnz_pam.conf
```
uncomment
LoadModule authnz_pam_module modules/mod_authnz_pam.so  
```
vi /etc/httpd/conf.d/authnz_pam.conf
```
add to the end  
```
<Directory "/var/www/html/protected">  
    AuthType Basic  
    AuthName "PAM Authentication"  
    AuthBasicProvider PAM  
    AuthPAMService httpd-auth  
    Require valid-user  
</Directory>  
```
```
vi /etc/pam.d/httpd-auth
```
create new
```
auth       required     pam_listfile.so item=user sense=deny file=/etc/httpd/conf.d/denyusers onerr=succeed
auth       include      system-auth
account    include      system-auth
```

```
chgrp apache /etc/shadow
chmod 440 /etc/shadow 
systemctl reload httpd
curl -u javed:TmPcZjtRQx http://localhost:8080/protected/
exit
```
[reference](https://www.server-world.info/en/note?os=CentOS_Stream_9&p=httpd&f=9)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 



The Nautilus devops team got some requirements related to some Apache config changes.  
They need to setup some redirects for some URLs. There might be some more changes need to be done. Below you can find more details regarding that:  
1.) httpd is already installed on app server 1. Configure Apache to listen on port 3000.  
Configure Apache to add some redirects as mentioned below:  
    a.) Redirect http://stapp01.stratos.xfusioncorp.com:<Port>/ to http://www.stapp01.stratos.xfusioncorp.com:<Port>/ i.e non www to www. This must be a permanent redirect i.e 301  
    b.) Redirect http://www.stapp01.stratos.xfusioncorp.com:<Port>/blog/ to http://www.stapp01.stratos.xfusioncorp.com:<Port>/news/. This must be a temporary redirect i.e 302.  

Solution:  
```
ssh tony@stapp01
sudo su
vi /etc/httpd/conf/http.conf
```
Then find LISTEN and change the port to 3000. 
```
vi /etc/httpd/conf.d/main.conf
```
and add:  
```
<VirtualHost *:3000>
    ServerName stapp01.stratos.xfusioncorp.com
    Redirect 301 / http://www.stapp01.stratos.xfusioncorp.com:3000/
</VirtualHost>

<VirtualHost *:3000>
    Redirect 302 /blog/ http://www.stapp01.stratos.xfusioncorp.com:3000/news/
</VirtualHost>
```
Then check the configuration:  
```
apachectl configtest
systemctl start httpd
systemctl status httpd
curl -I http://stapp01.stratos.xfusioncorp.com:3000/
curl -I http://www.stapp01.stratos.xfusioncorp.com:3000/blog/
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 



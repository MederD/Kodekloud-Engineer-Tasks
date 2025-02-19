xFusionCorp Industries has hosted several static websites on Nautilus Application Servers in Stratos DC. There are some confidential directories in the document root that need to be password protected.  
Since they are using Apache for hosting the websites, the production support team has decided to use .htaccess with basic auth.  
There is a website that needs to be uploaded to /var/www/html/finance on Nautilus App Server 3. However, we need to set up the authentication before that.  
1. Create /var/www/html/finance direcotry if doesn't exist.  
2. Add a user kirsty in htpasswd and set its password to GyQkFRVNr3.  
3. There is a file /tmp/index.html present on Jump Server. Copy the same to the directory you created, please make sure default document root should remain /var/www/html. Also website should work on URL http://<app-server-hostname>:8080/finance/

Solution:  
```bash
ssh banner@stapp03
sudo mkdir -p /var/www/html/finance
sudo htpasswd -c /etc/httpd/.htpasswd kirsty
exit
```
Copy file from jump_host:  
```bash
scp /tmp/index.html banner@stapp03:/tmp/
ssh banner@stapp03
sudo mv /tmp/index.html /var/www/html/finance
sudo systemctl enable httpd
sudo systemctl start httpd
sudo systemctl status httpd
exit
```
Check from jump_host:  
```bash
curl http://stapp03.stratos.xfusioncorp.com:8080/finance/
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

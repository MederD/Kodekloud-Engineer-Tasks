The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 3 in Stratos Datacenter.  
They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:  
1. Install and configure nginx on App Server 3.  
2. On App Server 3 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.  
3. Create an index.html file with content Welcome! under Nginx document root.  
4. For final testing try to access the App Server 3 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.

Solution:  
```bash
ssh banner@stapp03
sudo yum install nginx
sudo mv /tmp/nautilus.crt /tmp/nautilus.key /etc/ssl/
sudo openssl dhparam -out /etc/ssl/dhparam.pem 2048
sudo vi /etc/nginx/conf.d/ssl.conf
```
Add this:  
```
server {
    listen 443 http2 ssl;
    listen [::]:443 http2 ssl;

    server_name your_server_ip;

    ssl_certificate /etc/ssl/nautilus.crt;
    ssl_certificate_key /etc/ssl/nautilus.key;
    ssl_dhparam /etc/ssl/dhparam.pem;

    root /usr/share/nginx/html;

    location / {
    }

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}
```
Create `index.html` page:  
```bash
sudo echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
sudo nginx -t
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
exit
```
From jump_host test:  
```bash
thor@jumphost ~$ curl -Ik https://172.16.238.12/
HTTP/2 200 
server: nginx/1.20.1
date: Mon, 24 Feb 2025 21:36:53 GMT
content-type: text/html
content-length: 9
last-modified: Mon, 24 Feb 2025 21:35:53 GMT
etag: "67bce639-9"
accept-ranges: bytes

thor@jumphost ~$ curl -k https://172.16.238.12/
Welcome!
thor@jumphost ~$
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)   
[reference](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-centos-7)



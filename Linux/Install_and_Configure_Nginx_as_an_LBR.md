Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance.  
Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC.   
They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:  
a. Install nginx on LBR (load balancer) server.  
b. Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.  
c. Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.  
d. Once done, you can access the website using StaticApp button on the top bar.  

Solution:  
```
ssh tony@stapp01
sudo netstat -tulpn | grep http -> check for the apache port
ssh loki@stlb01
sudo yum install nginx -y
sudo systemctl start nginx && sudo systemctl enable nginx
sudo systemctl status nginx
sudo vi /etc/nginx/nginx.conf
```
add below:
```
http {
    upstream myapp1 {
        server stapp01.stratos.xfusioncorp.com:8084;
        server stapp02.stratos.xfusioncorp.com:8084;
        server stapp03.stratos.xfusioncorp.com:8084;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
        }
    }
}
sudo nginx -t -> check for configuration syntax
sudo systemctl reload nginx
```
[reference](https://nginx.org/en/docs/http/load_balancing.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

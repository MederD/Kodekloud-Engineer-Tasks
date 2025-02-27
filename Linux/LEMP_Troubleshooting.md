We have LEMP stack configured on apps and database server in Stratos DC. Its using Nginx + php-fpm, for now we have deployed a sample php page on these apps.  
Due to some misconfiguration the php page is not loading on the web server. Seems like at least two app servers are having issues. Find below more details and make sure website works on LBR URL and locally on each app as well.  
1. Nginx is supposed to run on port 80 on all app servers.  
2. Nginx document root is /var/www/html/  
3. Test the webpage on LBR URL (use LBR button on the top bar) and locally on each app server to make sure it works. It must not display any error message or nginx default page.

Solution:  
```
telnet stapp01 80
```
This will check if required port is open and working. I  my case stapp01 was not responding.
```
sudo systemctl status nginx
sudo cat /etc/nginx/nginx.conf
```
stapp01 wrong port was configured.  
This is the working configuration:  
```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /var/www/html;
        index index.php index.html index.htm;


        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

       location ~ .php$ {
           try_files $uri =404;
           fastcgi_pass unix:/var/opt/remi/php74/run/php-fpm/www.sock;
           fastcgi_index index.php;
           fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
           include fastcgi_params;
        }     
        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2 default_server;
#        listen       [::]:443 ssl http2 default_server;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers HIGH:!aNULL:!MD5;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        location / {
#        }
#
#        error_page 404 /404.html;
#        location = /404.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#        location = /50x.html {
#        }
#    }

}
```
Call `curl http://localhost` from every app server to make sure its working.

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

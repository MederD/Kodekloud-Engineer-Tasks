The Nautilus Application team is planning to deploy one of the php-based application on Kubernetes cluster. As per discussion with DevOps team they decided to use nginx and phpfpm. Additionally, they shared some custom configuration requirements. Below you can find more details. Please complete the task as per requirements mentioned below:  

1.) Create a config map nginx-config for nginx.conf as we want to add some custom settings for nginx.conf.  

a) Change default port 80 to 8096 in nginx.conf.  

b) Change default document root /usr/share/nginx to /var/www/html in nginx.conf.  

c) Update directory index to index index.html index.htm index.php in nginx.conf.  

2.) Create a pod named nginx-phpfpm.  

b) Create a shared files volume shared-files that will be used by both containers (nginx and phpfpm) also it should be emptyDir: {}  

c) Map the ConfigMap we declared above as a volume for the pod. Name the volume as nginx-config-volume  

d) Nginx container should be named as nginx-container and it should use nginx:latest image. PhpFPM container should be named as php-fpm-container and it should use php:7.1-fpm image.  

e) The shared volume shared-files should be mounted at /var/www/html location in both containers.  

f) Add one more volume for nginx-container. Name it as nginx-config-volume, mount path should be /etc/nginx/nginx.conf and subPath should be nginx.conf  

Before clicking on finish button always make sure to check if all pods are in running state.  

You can use any labels as per your choice.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  

```
apiVersion: v1
kind: ConfigMap
metadata:
   name: nginx-config
data:
   nginx.conf: |
      events {} 
      http {
         server {
            listen 8096;
            index index.html index.htm index.php;
            root /var/www/html;
            location ~ \.php$ {
               include fastcgi_params;
               fastcgi_param REQUEST_METHOD $request_method;
               fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
               fastcgi_pass 127.0.0.1:9000;
            }
         }
      }
---
apiVersion: v1
kind: Pod
metadata:
   name: nginx-phpfpm
spec:
   volumes:
      - name: shared-files
        emptyDir: {}
      - name: nginx-config-volume
        configMap:
            name: nginx-config
   containers:
      - name: nginx-container
        image: nginx:latest
        volumeMounts:
          - name: shared-files
            mountPath: /var/www/html
          - name: nginx-config-volume
            mountPath: /etc/nginx/nginx.conf
            subPath: nginx.conf
      - name: php-fpm-container
        image: php:7.1-fpm
        volumeMounts:
          - name: shared-files
            mountPath: /var/www/html
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

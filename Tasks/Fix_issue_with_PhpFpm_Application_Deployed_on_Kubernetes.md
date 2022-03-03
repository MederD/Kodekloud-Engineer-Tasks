We deployed a Nginx and PHPFPM based application on Kubernetes cluster last week and it had been working fine. This morning one of the team members was troubleshooting an issue with this stack and he was supposed to run Nginx welcome page for now on this stack till issue with phpfpm is fixed but he made a change somewhere which caused some issue and the application stopped working. Please look into the issue and fix the same:  

The deployment name is nginx-phpfpm-dp and service name is nginx-service. Figure out the issues and fix them. FYI Nginx is configured to use default http port, node port is 30008 and copy index.php under /tmp/index.php to deployment under /var/www/html. Please do not try to delete/modify any other existing components like deployment name, service name etc.
Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  

```
thor@jump_host ~$ kubectl get deploy
NAME              READY   UP-TO-DATE   AVAILABLE   AGE
nginx-phpfpm-dp   1/1     1            1           58s
```  

```  
thor@jump_host ~$ kubectl get svc
NAME            TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes      ClusterIP      10.96.0.1      <none>        443/TCP          145m
nginx-service   LoadBalancer   10.96.45.194   <pending>     8098:30008/TCP   69s
```  

```
thor@jump_host ~$ kubectl get cm
NAME               DATA   AGE
kube-root-ca.crt   1      145m
nginx-config       1      80s
```  

```
thor@jump_host ~$ kubectl describe cm nginx-config
Name:         nginx-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
nginx.conf:
----
events {
}
http {
  server {
    listen 80 default_server;
    listen [::]:80 default_server;

    # Set nginx to serve files from the shared volume!
    root /var/www/html;
    index  index.html index.ph p index.htm;
    server_name _;
    location / {
      try_files $uri $uri/ =404;
    }
    location ~ \.php$ {
      include fastcgi_params;
      fastcgi_param REQUEST_METHOD $request_method;
      fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
      fastcgi_pass 127.0.0.1:9000;
    }
  }
}
```  
Edit configmap and add index.php   
Then, edit service, change container ports to 80  

```
thor@jump_host ~$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-phpfpm-dp-5cccd45499-6j7l6   2/2     Running   0          3m32s
```  

Copy /tmp/index.php   
```
thor@jump_host ~$ kubectl cp /tmp/index.php nginx-phpfpm-dp-5cccd45499-6j7l6:/var/www/html -c nginx-container
```  

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  




One of the junior DevOps team members was working to deploy a stack on Kubernetes cluster. Somehow the pod is not coming up and is failing with some errors. We need to fix this as soon as possible. Please look into it.  

1.) There is a pod named webserver, the container under it named as httpd-container and it is using image httpd:latest  

2.) There is a sidecar container as well named sidecar-container which is using ubuntu:latest image.  

Look into the issue and fix it, make sure pod is in running state before clicking the Finish button.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:

Run this command to see the problem:  
```
kubectl describe pod webserver
```

In this case, image name was assigned with typo, fix "httpd:latests" to "httpd:latest":  
```
kubectl edit pod webserver
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

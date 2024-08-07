A junior DevOps team member encountered difficulties deploying a stack on the Kubernetes cluster. The pod fails to start, presenting errors. Let's troubleshoot and rectify the issue promptly.  

There is a pod named webserver, and the container within it is named httpd-container, its utilizing the httpd:latest image.  

Additionally, there's a sidecar container named sidecar-container using the ubuntu:latest image.  

Identify and address the issue to ensure the pod is in the running state and the application is accessible.  

Note: The kubectl utility on jump_host is configured to interact with the Kubernetes cluster.  

Solution:

```
kubectl edit pod webserver -o yaml

check the image name and fix it

save the file
```

[back](https://github.com/MederD) 

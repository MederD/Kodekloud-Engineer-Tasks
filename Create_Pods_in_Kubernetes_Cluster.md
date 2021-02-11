The Nautilus DevOps team has started practicing some pods and services deployment on Kubernetes platform as they are planning to migrate most of their applications on Kubernetes platform. Recently one of the team members has been assigned a task to create a pod as per details mentioned below:  

1.) Create a pod named pod-nginx using the image nginx with latest tag only and remember to mention tag i.e nginx:latest.  

2.) Labels app should be named as nginx_app, also container should be named as nginx-container.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:

Create pod.yml:  
```
apiVersion: v1
kind: Pod
metadata:
  name: pod-nginx
  labels:
  	 app: nginx_app
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - name: web
          containerPort: 80
          protocol: TCP
          
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest features to prod branch and those need be deployed. The Nautilus DevOps team has created an image nginx:1.17 with latest changes.  

Perform a rolling update for this application and incorporate nginx:1.17 image. The deployment name is nginx-deployment  

Make sure all pods are up and running after the update.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

1.)  Run this command: 
```
kubectl edit deployment nginx-deployment
```

2.) Change container image to - nginx:1.17   


3.) Check the status of rollout:  
```
kubectl rollout status deployment/nginx-deployment
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  


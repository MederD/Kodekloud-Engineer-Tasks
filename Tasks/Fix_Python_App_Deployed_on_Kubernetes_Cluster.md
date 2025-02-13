One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. 
Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.
```
    The deployment name is python-deployment-nautilus, its using poroko/flask-demo-appimage. The deployment and service of this app is already deployed.
    nodePort should be 32345 and targetPort should be python flask app's default port.
```
Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.  

Solution:  
```
kubectl describe pod `POD_NAME` | grep -i events -A 5
kubectl edit svc `SVC_NAME` -> change the target port to 5000
kubectl edit deploy `DEPLOYMENT_NAME` -> change image to poroko/flask-demo-app
https://hub.docker.com/r/poroko/flask-demo-app/tags
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

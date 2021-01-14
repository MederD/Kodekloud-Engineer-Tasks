There is an application deployed on Kubernetes cluster. Recently the Nautilus application development team developed a new version of the application that needs to be deployed now. As per new updates some new changes need to be made in this existing setup. So update the deployment and service as per details mentioned below:  

We already have a deployment named nginx-deployment and service named nginx-service. Some changes need to be made in this deployment and service, make sure not to delete the deployment and service.  

1.) Change the service nodeport from 30008 to 32165  
2.) Change replicas count from 1 to 5  
3.) Add a command: ["sh","-c","while true; do apt update; apt install -y vim php ; sleep 3; done"]  
4.) Change the image from nginx:1.17 to nginx:latest  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

```
kubectl edit services nginx-service
```
Change port 30008 to 32165 and to confirm run:  

```
kubectl get services
```

```
kubectl edit deployment nginx-deployment
```
Change image, replicas count and add command.  

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

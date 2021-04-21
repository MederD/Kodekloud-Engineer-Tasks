Some of the Nautilus team developers are developing a static website and they want to deploy it on Kubernetes cluster. They want it to be highly available and scalable. Therefore, based on the requirements, the DevOps team has decided to create deployment for it with multiple replicas. Below you can find more details about it:  

1.) Create a deployment using nginx image with latest tag only and remember to mention tag i.e nginx:latest and name it as nginx-deployment. App labels should be app: nginx-app and type: front-end. The container should be named as nginx-container; also make sure replica counts are 3.  

2.) Also create a service named nginx-service and type NodePort. The targetPort should be 80 and nodePort should be 30011.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-app
    type: front-end
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-app
      type: front-end
  template:
    metadata:
      labels:
        app: nginx-app
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
     app: nginx-app
     type: front-end
spec:
  type: NodePort
  selector:
     app: nginx-app
     type: front-end
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30011
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

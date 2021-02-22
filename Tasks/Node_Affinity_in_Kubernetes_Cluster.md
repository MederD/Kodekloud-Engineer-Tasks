The Nautilus DevOps team is doing some testing around the Kubernetes cluster for pods, deployments etc.  
A new ticket has been raised by one of the developer with some particular requirements. Find below more details and perform the task accordingly.  

1.) Create a label for node node01 color=grey,  

2.) Based on the new label created for node01 using nodeAffinity matchExpressions create a new deployment named grey, use image nginx with latest tag only and remember to mention tag i.e nginx:latest and 3 replicas; ensure container name is nginx-container and that it gets placed on the node01 node only. Also make sure all pods are in running state before clicking on FINISH.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  
1.) Assign label color=grey to the node01:  
```  
kubectl label node node01 color=grey  
```
We can verify if label created by running:  
```
kubectl get nodes --show-labels  
```

2.) Create deploy.yml   
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grey
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - grey
      containers:
      - name: nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
```
```
kubectl apply -f deploy.yml
```

Check the running pods if they are created on node01:  
```
kubectl get pods -o wide
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  



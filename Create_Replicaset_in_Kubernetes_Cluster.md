The Nautilus DevOps team is going to deploy some applications on kubernetes cluster as they are planning to migrate some of their applications there.  
Recently one of the team members has been assigned a task to write a template as per details mentioned below:  

1.Create a ReplicaSet using nginx image with latest tag only and remember to mention tag i.e nginx:latest and name it as nginx-replicaset.  

2.Labels app should be nginx_app, labels type should be front-end. The container should be named as nginx-container; also make sure replicas counts are 4.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    app: nginx_app
    type: front-end
spec:
  replicas: 4
  selector:
    matchLabels:
      type: front-end
  template:
    metadata:
      labels:
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        
```

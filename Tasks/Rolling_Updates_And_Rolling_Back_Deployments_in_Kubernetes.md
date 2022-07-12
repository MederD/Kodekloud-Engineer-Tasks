There is a production deployment planned for next week. The Nautilus DevOps team wants to test the deployment update and rollback on Dev environment first so that they can identify the risks in advance. Below you can find more details about the plan they want to execute.

    Create a namespace nautilus. Create a deployment called httpd-deploy under this new namespace, It should have one container called httpd, use httpd:2.4.27 image and 2 replicas. The deployment should use RollingUpdate strategy with maxSurge=1, and maxUnavailable=2. Also create a NodePort type service named httpd-service and expose the deployment on nodePort: 30008.

    Now upgrade the deployment to version httpd:2.4.43 using a rolling update.

    Finally, once all pods are updated undo the recent update and roll back to the previous/original version.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

Solution:  

Create deploy.yaml:  
```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deploy
  namespace: nautilus
  labels:
    app: httpd
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
  replicas: 2
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd
        image: httpd:2.4.27

---
apiVersion: v1
kind: Service
metadata:
  name: httpd-service
  namespace: nautilus
  labels:
     app: httpd
spec:
  type: NodePort
  selector:
     app: httpd
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30008
```

Run command:  
```
kubectl apply -f deploy.yaml
```

After pods are created, run:  
```
kubectl set image deployment httpd-deploy -n nautilus httpd=httpd:2.4.43 --record
```

Check the rollout status and history:  
```
kubectl rollout status deployment httpd-deploy -n nautilus
```
```
kubectl rollout history deployment httpd-deploy -n nautlus
```

then, rollback to revision 1:  
```
kubectl rollout undo deployment httpd-deploy -n nautilus --to-revision=1
```

Use for reference: https://phoenixnap.com/kb/kubernetes-rolling-update   

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)    

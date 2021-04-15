The Nautilus DevOps teams is planning to set up a Nagios monitoring tool to monitor some applications, services etc. They are planning to deploy it on Kubernetes cluster. Below you can find more details.   

1) Create a deployment nagios-deployment for Nagios core. The container name must be nagios-container and it must use jasonrivers/nagios image.   

2) Create a user and password for the Nagios core web interface, user must be xFusionCorp and password must be LQfKeWWxWD. (you can manually perform this step after deployment)   

3) Create a service nagios-service for Nagios which must be of targetPort type. Port must be 80 and nodePort must be 30008.    

You can use any labels as per your choice.   

Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.    

Solution:  
Create deploy.yml   

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nagios-deployment
  labels:
    app: nagios-app
    name: nagios-deployment
spec:
  selector:
    matchLabels:
      app: nagios-app
      name: nagios-pod
  template:
    metadata:
      labels:
        app: nagios-app
        name: nagios-pod
    spec:
      containers:
      - name: nagios-container
        image: jasonrivers/nagios
        ports:
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: nagios-service
  labels:
    name: nagios-service
    app: nagios-app
spec:
  type: NodePort
  selector:
     name: nagios-pod
     app: nagios-app
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30008
```

Run:  
```
kubectl apply -f deploy.yml
```

Reset nagios password:  
```
kubectl exec -it <pod-name> --/bin/bash
```

Find the /nagios/etc/htpasswd.users file and run this command:    
```
htpasswd /nagios/etc/htpasswd.users xFusionCorp
```
You will be prompted to enter new password, which is: LQfKeWWxWD   

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

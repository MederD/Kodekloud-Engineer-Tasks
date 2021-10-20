The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.  

1.) Create a deployment named grafana-deployment-devops using any grafana image for Grafana app. Set other parameters as per your choice.  

2.) Create NodePort type service with nodePort 32000 to expose the app.  

You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page.  

Note: The kubeclt on jump_host has been configured to work with kubernetes cluster.  

Solution:  
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      name: grafana
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana:latest
        ports:
        - name: grafana
          containerPort: 3000
---

apiVersion: v1
kind: Service
metadata:
  name: grafana
spec:
  selector: 
    app: grafana
  type: NodePort  
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

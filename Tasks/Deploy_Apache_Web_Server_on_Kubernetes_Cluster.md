There is an application that needs to be deployed on Kubernetes cluster under Apache web server. The Nautilus application development team has asked the DevOps team to deploy it. We need to develop a template as per requirements mentioned below:  

1.) Create a namespace named as httpd-namespace-xfusion.  

2.) Create a service named as httpd-service-xfusion under same namespace, targetPort should be 80 and nodePort should be 30004.  

3.) Create a deployment named as httpd-deployment-xfusion under the same namespace as mentioned above. Use image httpd with latest tag only and remember to mention tag i.e httpd:latest, and container name should be httpd-container-xfusion. And make sure replicas counts are 4.   

4.) Set labels app to httpd_app_xfusion.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.   

Solution:  

```
apiVersion: v1
kind: Namespace
metadata: 
  name: httpd-namespace-xfusion

---
apiVersion: v1
kind: Service
metadata:
  name: httpd-service-xfusion
  namespace: httpd-namespace-xfusion
  labels:
     app: httpd_app_xfusion
spec:
  type: NodePort
  selector:
     app: httpd_app_xfusion
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30004
    
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deployment-xfusion
  namespace: httpd-namespace-xfusion
  labels:
    app: httpd_app_xfusion
spec:
  replicas: 4
  selector:
    matchLabels:
      app: httpd_app_xfusion
  template:
    metadata:
      labels:
        app: httpd_app_xfusion
    spec:
      containers:
      - name: httpd-container-xfusion
        image: httpd:latest
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

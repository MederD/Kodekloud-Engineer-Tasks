A new java-based application is ready to be deployed on a Kubernetes cluster. The development team had a meeting with the DevOps team share requirements and application scope. The team is ready to setup an application stack for it under their existing cluster. Below you can find the details for this:  

1.) Create a namespace named tomcat-namespace-nautilus.  

2.) Create a deployment for tomcat app which should be named tomcat-deployment-nautilus under the same namespace you created. Replicas count should be 1, the container should be named as tomcat-container-nautilus, its image should be gcr.io/kodekloud/centos-ssh-enabled:tomcat and its container port should be 8080.  

3.) Create a service for tomcat app which should be named as tomcat-service-nautilus under the same namespace you created. Service type should be NodePort. Port's protocol should be TCP, port should be 80, targetPort should be 8080 and nodePort should be 32227.  

Before clicking on Finish button please make sure the application is up and running.  

You can use any labels as per your choice.  

Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.  

Solution:  

1.) Run:
```
kubectl create namespace dev
```

2.) Create deploy.yaml, with below configuration:  
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat-deployment-nautilus
  namespace: tomcat-namespace-nautilus
  labels:
    app: tomcat-deployment-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat-deployment-nautilus
  template:
    metadata:
      labels:
        app: tomcat-deployment-nautilus
    spec:
      containers:
      - name: tomcat-container-nautilus
        image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
        ports:
        - containerPort: 8080
        
---        
apiVersion: v1
kind: Service
metadata:
  name: tomcat-service-nautilus
  namespace: tomcat-namespace-nautilus
spec:
  type: NodePort
  selector:
    app: tomcat-deployment-nautilus
  ports:
    - port: 80
      protocol: TCP
      targetPort: 8080
      nodePort: 32227
```

3.) Run:
```
kubectl apply -f deploy.yaml
```

4.) You can check deployments:  
```
kubectl get pods --all-namespaces
```
```
kubectl get svc --all-namespaces
```
```
kubectl get deployments --all-namespaces
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  


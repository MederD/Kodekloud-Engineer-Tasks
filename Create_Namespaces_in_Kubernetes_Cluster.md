The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform. The team has already set up a Kubernetes cluster and now they want set up some namespaces, deployments etc. Based on the current requirements the team has shared the details below:  

Create a namespace named dev and create a POD under it; name the pod dev-nginx-pod and use nginx image with latest tag only and remember to mention tag i.e nginx:latest.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:

1.) Let's create namespace:  
```
kubectl create namespace dev
```

2.) Create pod.yml:  
```
apiVersion: v1
kind: Pod
metadata:
  name: dev-nginx-pod
  namespace: dev
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - name: web
          containerPort: 80
          protocol: TCP
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

The Nautilus DevOps team is running a voting app on their old infrastructure that they want to migrate to the Kubernetes cluster. They have already prepared the requirements related to the application stack. As per details given below deploy the voting app on the Kubernetes cluster:  

1.) Create a namespace which should be named vote.  

2.) Create a service vote-service under the namespace you created. Port should be 5000, targetPort should be 80 and nodePort should be 31000. Service endpoint exposes deployment vote-deployment under namespace vote.  

3.) Create a deployment vote-deployment under the namespace you created. Replicas count should be 1, the container name should be vote, use kodekloud/examplevotingapp_vote:before image ( use exact name of image as mentioned ) and containerPort should be 80.  

4.) Create a service redis under the same namespace. Port should be 6379, targetPort should be 6379, its type should be ClusterIP service endpoint exposes deployment redis-deployment under namespace vote.  

5.) Create a redis deployment redis-deployment under the same namespace. Replica count should be 1. The container name should be redis use redis:alpine image ( use exact name as mentioned ). Volume type should be EmptyDir, volumeName should be redis-data and its mountPath should be /data  

6.) Create a deployment worker under the same namespace. Its replicas count should be 1. The container name should be worker, use kodekloud/examplevotingapp_worker image ( use exact name of image as mentioned ).  

7.) Create a service db under the same namespace. Port shold be 5432, targetPort should be 5432 and its type should be ClusterIP.  

8.) Create a deployment db-deployment under the same namespace. Its replica count should be 1. The container name should be postgres, use postgres:9.4 image ( use exact name of image as mentioned ). Define ENV as first env name should be POSTGRES_USER and its value should be postgres, second env name should be POSTGRES_PASSWORD and its value should be postgres and third env name should be POSTGRES_HOST_AUTH_METHOD and its value should be trust. Volume type should be EmptyDir, volumeName should be db-data and its mountPath should be /var/lib/postgresql/data.  

9.) Create a deployment for the result which should be named result-deployment under the same namespace. Configure spec as replica should be 1. The container name should be result, use image kodekloud/examplevotingapp_result:before ( use exact name of image as mentioned ) and the containerPort should be 80.   

10.) Create a service for result which should be named as result-service under the same namespace. Configure spec as port should be 5001, targetPort should be 80 and nodePort should be 31001 and its type should be NodePort.  

You can use any labels as per your choice.  

Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.  

Solution:  
```
apiVersion: v1
kind: Namespace
metadata: 
  name: vote

---
apiVersion: v1
kind: Service
metadata:
  name: vote-service
  namespace: vote
  labels:
    name: vote-service
    app: voting-app
spec:
  type: NodePort
  selector:
     name: vote-pod
     app: voting-app
  ports:
  - port: 5000
    targetPort: 80
    nodePort: 31000
      
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: vote-deployment
  namespace: vote
  labels:
    app: voting-app
    name: vote-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: voting-app
      name: vote-pod
  template:
    metadata:
      labels:
        app: voting-app
        name: vote-pod
    spec:
      containers:
      - name: vote
        image: kodekloud/examplevotingapp_vote:before
        ports:
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: redis
  namespace: vote
spec:
  type: ClusterIP
  selector:
    app: voting-app
    name: redis-pod
  ports:
    - name: client
      port: 6379
      targetPort: 6379

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
  namespace: vote
  labels:
    app: voting-app
    name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: voting-app
      name: redis-pod
  template:
    metadata:
      labels:
        app: voting-app
        name: redis-pod
    spec:
      containers:
      - name: redis
        image: redis:alpine
        volumeMounts:
        - name: redis-data
          mountPath: /data
      volumes:
      - name: redis-data
        emptyDir: {}       

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: worker
  namespace: vote
  labels:
    app: voting-app
    name: worker
spec:
  replicas: 1
  selector:
    matchLabels:
      app: voting-app
      name: worker-pod
  template:
    metadata:
      labels:
        app: voting-app
        name: worker-pod
    spec:
      containers:
      - name: worker
        image: kodekloud/examplevotingapp_worker

---
apiVersion: v1
kind: Service
metadata:
  name: db
  namespace: vote
  labels:
    name: postgres-service
    app: voting-app
spec:
  type: ClusterIP
  selector:
    app: voting-app
    name: db-pod
  ports:
  - port: 5432
    targetPort: 5432

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: db-deployment
  namespace: vote
  labels:
    app: voting-app
    name: db-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: voting-app
      name: db-pod
  template:
    metadata:
      labels:
        app: voting-app
        name: db-pod
    spec:
      containers:
      - name: postgres
        image: postgres:9.4
        env:
        - name: POSTGRES_USER
          value: "postgres"
        - name:  POSTGRES_PASSWORD
          value: "postgres"
        - name: POSTGRES_HOST_AUTH_METHOD
          value: trust
        volumeMounts:
        - name: db-data
          mountPath: /var/lib/postgresql/data
      volumes:
      - name: db-data
        emptyDir: {}
    
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: result-deployment
  namespace: vote
  labels:
    app: voting-app
    name: result-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: voting-app
      name: result-pod
  template:
    metadata:
      labels:
        app: voting-app
        name: result-pod
    spec:
      containers:
      - name: result
        image: kodekloud/examplevotingapp_result:before
        ports:
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: result-service
  namespace: vote
  labels:
    name: result-service
    app: voting-app
spec:
  type: NodePort
  selector:
    app: voting-app
    name: result-pod
  ports:
  - port: 5001
    targetPort: 80
    nodePort: 31001

```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

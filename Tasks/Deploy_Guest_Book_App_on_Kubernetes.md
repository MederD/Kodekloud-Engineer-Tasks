The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.  

BACK-END TIER  
```
    Create a deployment named redis-master for Redis master.

    a.) Replicas count should be 1.

    b.) Container name should be master-redis-nautilus and it should use image redis.

    c.) Request resources as CPU should be 100m and Memory should be 100Mi.

    d.) Container port should be redis default port i.e 6379.

    Create a service named redis-master for Redis master. Port and targetPort should be Redis default port i.e 6379.

    Create another deployment named redis-slave for Redis slave.

    a.) Replicas count should be 2.

    b.) Container name should be slave-redis-nautilus and it should use gcr.io/google_samples/gb-redisslave:v3 image.

    c.) Requests resources as CPU should be 100m and Memory should be 100Mi.

    d.) Define an environment variable named GET_HOSTS_FROM and its value should be dns.

    e.) Container port should be Redis default port i.e 6379.

    Create another service named redis-slave. It should use Redis default port i.e 6379.
```  
FRONT END TIER  
```
    Create a deployment named frontend.

    a.) Replicas count should be 3.

    b.) Container name should be php-redis-nautilus and it should use gcr.io/google-samples/gb-frontend:v4 image.

    c.) Request resources as CPU should be 100m and Memory should be 100Mi.

    d.) Define an environment variable named as GET_HOSTS_FROM and its value should be dns.

    e.) Container port should be 80.

    Create a service named frontend. Its type should be NodePort, port should be 80 and its nodePort should be 30009.
```  
Finally, you can check the guestbook app by clicking on + button in the top left corner and Select port to view on Host 1 then enter your nodePort.  

You can use any labels as per your choice.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
  labels:
    app: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: master-redis-nautilus
        image: redis
        ports:
        - containerPort: 6379
        resources:
          requests:
            memory: "100Mi"
            cpu: "100m"
---
apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  selector:
    app: redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
  labels:
    app: redis-slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis-slave
  template:
    metadata:
      labels:
        app: redis-slave
    spec:
      containers:
      - name: slave-redis-nautilus
        image: gcr.io/google_samples/gb-redisslave:v3
        ports:
        - containerPort: 6379
        resources:
          requests:
            memory: "100Mi"
            cpu: "100m"
        env:
        - name: GET_HOSTS_FROM
          value: "dns"
---
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
  selector:
    app: redis-slave
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: php-redis-nautilus
        image: gcr.io/google-samples/gb-frontend:v4
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "100Mi"
            cpu: "100m"
        env:
        - name: GET_HOSTS_FROM
          value: "dns"
---
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30009

```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)





There is an iron gallery app that the Nautilus DevOps team was developing. They have recently customized the app and are going to deploy the same on the Kubernetes cluster. Below you can find more details:  
```
Create a namespace iron-namespace-devops  
Create a deployment iron-gallery-deployment-devops for iron gallery under the same namespace you created.  
:- Labels run should be iron-gallery.
:- Replicas count should be 1.
:- Selector's matchLabels run should be iron-gallery.
:- Template labels run should be iron-gallery under metadata.
:- The container should be named as iron-gallery-container-devops, use kodekloud/irongallery:2.0 image ( use exact image name / tag ).
:- Resources limits for memory should be 100Mi and for CPU should be 50m.
:- First volumeMount name should be config, its mountPath should be /usr/share/nginx/html/data.
:- Second volumeMount name should be images, its mountPath should be /usr/share/nginx/html/uploads.
:- First volume name should be config and give it emptyDir and second volume name should be images, also give it emptyDir.
Create a deployment iron-db-deployment-devops for iron db under the same namespace.
:- Labels db should be mariadb.
:- Replicas count should be 1.
:- Selector's matchLabels db should be mariadb.
:- Template labels db should be mariadb under metadata.
:- The container name should be iron-db-container-devops, use kodekloud/irondb:2.0 image ( use exact image name / tag ).
:- Define environment, set MYSQL_DATABASE its value should be database_web, set MYSQL_ROOT_PASSWORD and MYSQL_PASSWORD value should be with some complex passwords for DB connections, and MYSQL_USER value should be any custom user ( except root ).
:- Volume mount name should be db and its mountPath should be /var/lib/mysql. Volume name should be db and give it an emptyDir.

Create a service for iron db which should be named iron-db-service-devops under the same namespace. Configure spec as selector's db should be mariadb.  
Protocol should be TCP, port and targetPort should be 3306 and its type should be ClusterIP.
Create a service for iron gallery which should be named iron-gallery-service-devops under the same namespace. Configure spec as selector's run should be iron-gallery.  
Protocol should be TCP, port and targetPort should be 80, nodePort should be 32678 and its type should be NodePort.
```
Note:
We don't need to make connection b/w database and front-end now, if the installation page is coming up it should be enough for now.
The kubectl on jump_host has been configured to work with the kubernetes cluster.  

Solution:  
```
kubectl create namespace iron-namespace-devops
kubectl set-context --current --namespace iron-namespace-devops
kubectl create secret --namespace iron-namespace-devops generic mysql-root-pass --from-literal=password=SomeR00tPassword
kubectl create secret --namespace iron-namespace-devops generic mysql-user-pass --from-literal=password=SomeUs3rPassword --from-literal=username=iron
kubectl create secret --namespace iron-namespace-devops generic mysql-db-url --from-literal=database=database_web
```
create deploy.yml file:  
```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-devops
  namespace: iron-namespace-devops
  labels:
    run: iron-gallery
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      volumes:
        - name: config
          emptyDir: {}
        - name: images
          emptyDir: {}
      containers:
        - name: iron-gallery-container-devops
          image: kodekloud/irongallery:2.0
          ports:
          - containerPort: 80
          volumeMounts:
          - name: config
            mountPath: /usr/share/nginx/html/data
          - name: images
            mountPath: /usr/share/nginx/html/uploads
          resources:
            limits:
              memory: "100Mi"
              cpu: "50m"

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-devops
  namespace: iron-namespace-devops
  labels:
    db: mariadb
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb 
  template:
    metadata:
      labels:
        db: mariadb 
    spec:
      volumes:
        - name: db
          emptyDir: {}
      containers:
        - name: iron-db-container-devops
          image: kodekloud/irondb:2.0
          ports:
          - containerPort: 3306
          volumeMounts:
          - name: db
            mountPath: /var/lib/mysql
          env:
          - name: MYSQL_ROOT_PASSWORD
            valueFrom:
              secretKeyRef:
                name: mysql-root-pass
                key: password
          - name: MYSQL_DATABASE
            valueFrom:
              secretKeyRef:
                name: mysql-db-url
                key: database
          - name: MYSQL_USER
            valueFrom:
              secretKeyRef:
                name: mysql-user-pass
                key: username
          - name: MYSQL_PASSWORD
            valueFrom:
              secretKeyRef:
                name: mysql-user-pass
                key: password
          
---
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-devops
  namespace: iron-namespace-devops
spec:
  type: NodePort
  selector:
    run: iron-gallery
  ports:
    - port: 80
      targetPort: 80
      nodePort: 32678
---
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-devops
  namespace: iron-namespace-devops
spec:
  selector:
    db: mariadb
  ports:
  - name: mysql
    protocol: TCP
    port: 3306
    targetPort: 3306
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)


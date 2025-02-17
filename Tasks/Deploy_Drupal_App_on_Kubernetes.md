We need to deploy a Drupal application on Kubernetes cluster. The Nautilus application development team want to setup a fresh Drupal as they will do the installation on their own. Below you can find the requirements, they have shared with us.  
1) Configure a persistent volume drupal-mysql-pv with hostPath = /drupal-mysql-data (/drupal-mysql-data directory already exists on the worker Node i.e jump host), 5Gi of storage and ReadWriteOnce access mode.
2) Configure one PersistentVolumeClaim named drupal-mysql-pvc with storage request of 3Gi and ReadWriteOnce access mode.
3) Create a deployment drupal-mysql with 1 replica, use mysql:5.7 image. Mount the claimed PVC at /var/lib/mysql.
4) Create a deployment drupal with 1 replica and use drupal:8.6 image.
4) Create a NodePort type service which should be named as drupal-service and nodePort should be 30095.
5) Create a service drupal-mysql-service to expose mysql deployment on port 3306.
6) Set rest of the configration for deployments, services, secrets etc as per your preferences. At the end you should be able to access the Drupal installation page by clicking on App button.
Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.

Solution:
```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: mysql-root-pass
type: kubernetes.io/basic-auth
stringData:
  password: YUIidhb667

---  
apiVersion: v1
kind: Secret
metadata:
  name: mysql-user-pass
type: kubernetes.io/basic-auth
stringData:
  username: kodekloud_tim
  password: B4zNgHA7Ya

---  
apiVersion: v1
kind: Secret
metadata:
  name: mysql-db-url
type: Opaque
stringData:
  database: kodekloud_db1

---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: drupal-mysql-pv
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/drupal-mysql-data"

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: drupal-mysql-pvc
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: drupal-mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - image: mysql:5.7
        name: mysql
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
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
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: drupal-mysql-pvc

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: drupal
spec:
  replicas: 1
  selector:
    matchLabels:
      app: drupal
  template:
    metadata:
      labels:
        app: drupal
    spec:
      containers:
      - image: drupal:8.6
        name: drupal
        ports: 
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: drupal-mysql-service
spec:
  ports:
  - port: 3306
    targetPort: 3306
  selector:
    app: mysql

---
apiVersion: v1
kind: Service
metadata:
  name: drupal-service
spec:
  type: NodePort
  selector:
    app: drupal
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30095
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

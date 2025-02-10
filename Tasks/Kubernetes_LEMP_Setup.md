The Nautilus DevOps team want to deploy a static website on Kubernetes cluster. They are going to use Nginx, phpfpm and MySQL for the database.  
The team had already gathered the requirements and now they want to make this website live. Below you can find more details:  
Create some secrets for MySQL.
Create a secret named `mysql-root-pass` wih key/value pairs as below:  
```
name: password
value: R00t
```
Create a secret named `mysql-user-pass` with key/value pairs as below:  
```
name: username
value: kodekloud_pop
name: password
value: YchZHRcLkL
```
Create a secret named `mysql-db-url` with key/value pairs as below:  
```
name: database
value: kodekloud_db8
```
Create a secret named `mysql-host` with key/value pairs as below:  
```
name: host
value: mysql-service
```
Create a config map `php-config` for php.ini with variables_order = "EGPCS" data.   
Create a deployment named `lemp-wp`.   
Create two containers under it. First container must be `nginx-php-container` using image webdevops/php-nginx:alpine-3-php7 and second container must be `mysql-container` from image mysql:5.6.  
Mount php-config configmap in nginx container at /opt/docker/etc/php/php.ini location.  
5) Add some environment variables for both containers:  
`MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD and MYSQL_HOST`. Take their values from the secrets you created. Please make sure to use env field (do not use envFrom) to define the name-value pair of environment variables.
6) Create a node port type service `lemp-service` to expose the web application, nodePort must be 30008.  
7) Create a service for mysql named `mysql-service` and its port must be 3306.  
We already have a `/tmp/index.php` file on jump_host server.
Copy this file into the nginx container under document root i.e `/app` and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters.  
Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.  
Once done, you must be able to access this website using Website button on the top bar, please note that you should see `Connected successfully` message while accessing this page.  
Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.  

Solution:  
1. Create secrets:  
```
kubectl create secret generic mysql-root-pass \
--from-literal=password=R00t \
```
2. Then, create deploy.yaml:  
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
   name: php-config
data:
   php.ini: |
     variables_order = "EGPCS" 

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: lemp-wp
  labels:
    app: lemp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: lemp
  template:
    metadata:
      labels:
        app: lemp
    spec:
      volumes:
        - name: php-config
          configMap:
            name: php-config
      containers:
        - name: nginx-php-container
          image: webdevops/php-apache:alpine-3-php7
          ports:
          - containerPort: 80
          volumeMounts:
          - name: php-config
            mountPath: /opt/docker/etc/php/php.ini
            subPath: php.ini
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
          - name: MYSQL_HOST
            valueFrom:
              secretKeyRef:
                name: mysql-host
                key: host
        - name: mysql-container
          image: mysql:5.6
          ports: 
          - containerPort: 3306
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
          - name: MYSQL_HOST
            valueFrom:
              secretKeyRef:
                name: mysql-host
                key: host
---
apiVersion: v1
kind: Service
metadata:
  name: lemp-service
spec:
  type: NodePort
  selector:
    app: lemp
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
---
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  selector:
    app: lemp
  ports:
  - name: mysql
    protocol: TCP
    port: 3306
    targetPort: 3306
```
3. After deployment the above file, and making sure the pods are running, change the `/tmp/index.php` and copy over to the pod:
```
<?php
$dbname = $_ENV["MYSQL_DATABASE"];
$dbuser = $_ENV["MYSQL_USER"];
$dbpass = $_ENV["MYSQL_PASSWORD"];
$dbhost = $_ENV["MYSQL_HOST"];

$connect = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($test_query);

if ($result->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}
  echo "Connected successfully";
```
4. Get the right pod name, in my case it is `lamp-wp-556dfb9b4c-x5tg4`:
```
kubectl cp /tmp/index.php lamp-wp-556dfb9b4c-x5tg4:/app -c nginx-php-container
```
5. Check if website is working. It should return "Connected successfully".

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

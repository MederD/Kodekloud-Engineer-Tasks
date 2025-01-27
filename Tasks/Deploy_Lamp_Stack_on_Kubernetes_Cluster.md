The Nautilus DevOps team want to deploy a PHP website on Kubernetes cluster. They are going to use Apache as a web server and Mysql for database.  
The team had already gathered the requirements and now they want to make this website live. Below you can find more details:  
```
1) Create a config map php-config for php.ini with variables_order = "EGPCS" data.
2) Create a deployment named lamp-wp.
3) Create two containers under it. First container must be httpd-php-container using image webdevops/php-apache:alpine-3-php7 and second container must be mysql-container from image mysql:5.6.
Mount php-config configmap in httpd container at /opt/docker/etc/php/php.ini location.
4) Create kubernetes generic secrets for mysql related values like myql root password, mysql user, mysql password, mysql host and mysql database. Set any values of your choice.
5) Add some environment variables for both containers:  
  a) MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD and MYSQL_HOST. Take their values from the secrets you created.
Please make sure to use env field (do not use envFrom) to define the name-value pair of environment variables.
6) Create a node port type service lamp-service to expose the web application, nodePort must be 30008.
7) Create a service for mysql named mysql-service and its port must be 3306.
8) We already have /tmp/index.php file on jump_host server.
  a) Copy this file into httpd container under Apache document root i.e /app and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters.
Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.
  b) You must be able to access this index.php on node port 30008 at the end, please note that you should see Connected successfully message while accessing this page.
Note:
The kubectl utility on jump_host has been configured to work with the kubernetes cluster.
```

Solution:  
1. Create secrets:  
```
kubectl create secret generic mysql-secrets \
--from-literal=MYSQL_ROOT_PASSWORD=admin \
--from-literal=MYSQL_DATABASE=database \
--from-literal=MYSQL_USER=user \
--from-literal=MYSQL_PASSWORD=password \
--from-literal=MYSQL_HOST=mysql-service
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
  name: lamp-wp
  labels:
    app: lamp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: lamp
  template:
    metadata:
      labels:
        app: lamp
    spec:
      volumes:
        - name: php-config
          configMap:
            name: php-config
      containers:
        - name: httpd-php-container
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
                name: mysql-secrets
                key: MYSQL_ROOT_PASSWORD 
          - name: MYSQL_DATABASE
            valueFrom:
              secretKeyRef:
                name: mysql-secrets
                key: MYSQL_DATABASE
          - name: MYSQL_USER
            valueFrom:
              secretKeyRef:
                name: mysql-secrets
                key: MYSQL_USER
          - name: MYSQL_PASSWORD
            valueFrom:
              secretKeyRef:
                name: mysql-secrets
                key: MYSQL_PASSWORD
          - name: MYSQL_HOST
            valueFrom:
              secretKeyRef:
                name: mysql-secrets
                key: MYSQL_HOST
        - name: mysql-container
          image: mysql:5.6
          ports: 
          - containerPort: 3306
          env:
          - name: MYSQL_ROOT_PASSWORD
            valueFrom:
              secretKeyRef:
                name: mysql-secrets
                key: MYSQL_ROOT_PASSWORD
          - name: MYSQL_DATABASE
            valueFrom:
              secretKeyRef:
                name: mysql-secrets
                key: MYSQL_DATABASE
          - name: MYSQL_USER
            valueFrom:
              secretKeyRef:
                name: mysql-secrets
                key: MYSQL_USER
          - name: MYSQL_PASSWORD
            valueFrom:
              secretKeyRef:
                name: mysql-secrets
                key: MYSQL_PASSWORD
          - name: MYSQL_HOST
            valueFrom:
              secretKeyRef:
                name: mysql-secrets
                key: MYSQL_HOST
---
apiVersion: v1
kind: Service
metadata:
  name: lamp-service
spec:
  type: NodePort
  selector:
    app: lamp
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
    app: lamp
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
kubectl cp /tmp/index.php lamp-wp-556dfb9b4c-x5tg4:/app -c httpd-php-container
```
5. Check if website is working. It should return "Connected successfully".

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  


1. The Nautilus application development team aims to test a Pod creation by creating a httpd based Pod on the Kubernetes cluster. The specifications for the same are as follows:  
  Create a Pod named httpd-test-t1q4 using httpd:alpine3.19 image, it must have a label app: httpd

**Solution:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-test-t1q4
  labels:
    app: httpd
spec:
  containers:
  - name: httpd
    image: httpd:alpine3.19
    ports:
    - containerPort: 80
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
2. In certain cases, applications deployed on the Kubernetes cluster require specific configurations or setup changes before launching the app container. The Nautilus DevOps team has devised a solution using init containers to fulfill these prerequisites during deployment. Below is an initial test scenario:  
a. Create a Pod named red-datacenter-t1q5. It must have an init container named red-init-datacenter-t1q5, it should utilise image centos (preferably with the latest tag). The command '/bin/bash', '-c' should be used with arguments echo "Welcome!"  
b. The main container name should be red-main-datacenter-t1q5 and it should utilise image centos (preferably with the latest tag). The Command: '/bin/bash', '-c' should be used with arguments sleep 1000
This scenario demonstrates the use of init containers to fulfil pre-requisites before deploying the main application container in the Kubernetes Pod.

**Solution:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: red-datacenter-t1q5
  labels:
    app: httpd
spec:
  initContainers:
  - name: red-init-datacenter-t1q5
    image: centos:latest
    command: ['bin/bash', '-c', 'echo "Welcome!"']
  containers:
  - name: red-main-datacenter-t1q5
    image: centos:latest
    command: ['bin/bash', '-c', 'sleep 1000']
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
3. The Nautilus devops team found that one of the applications that is deployed on the cluster is having some performance issues, they want to make some changes so that it can handle some more traffic. As per new updates some new changes need to be made in this existing setup. So update the deployment as per details mentioned below:
The deployment name is blue-app-t2q5, change its replicas count from 1 to 3.

**Solution:**
```bash
kubectl edit deployment blue-app-t2q5
then change the replciase from 1 to 3
or
kubectl scale --current-replicas=1 --replicas=3 deployment/blue-app-t2q5
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
4. This morning the Nautilus DevOps team rolled out a new release for one of the applications. Recently one of the customers logged a complaint which seems to be about a bug related to the recent release. Therefore, the team wants to rollback the recent release.  
There is a deployment named nginx-deployment-t2q2, roll it back to the previous revision.

**Solution:**
```bash
kubectl rollout undo deployment/nginx-deployment-t2q2
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
5. The Nautilus DevOps team aims to deploy microservices on the Kubernetes platform. They've established a Kubernetes cluster and are now focused on configuring namespaces, deployments, and related structures to meet their current needs. Here are the specific details shared by the team:  
Create a namespace named dev-t3q3.

**Solution:**
```bash
kubectl create namespace dev-t3q3
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
6. The Nautilus DevOps team is in the process of developing scripts to be executed on various schedules. Currently, they are provisioning cron jobs within the Kubernetes cluster with placeholder commands (to be substituted with actual scripts). Below are the specifications for creating a cronjob:  
a. Create a cronjob named datacenter-t3q1.  
b. Set Its schedule to something like */6 * * * *, you set any schedule for now.  
c. Container name should be cron-datacenter-t3q1.  
d. Use httpd image with latest tag only and remember to mention the tag i.e httpd:latest.  
e. Run a dummy command echo Welcome to xfusioncorp!.  
f. Ensure restart policy is OnFailure.

**Solution:**
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: datacenter-t3q1
spec:
  schedule: "*/6 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cron-datacenter-t3q1
            image: httpd:latest
            imagePullPolicy: IfNotPresent
            command:
            - /bin/bash
            - -c
            - echo Welcome to xfusioncorp!
          restartPolicy: OnFailure
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
7. We encountered complications while deploying an app on the Kubernetes cluster, resulting in its improper functionality. Your task is to investigate and restore its proper operation.  
App deployment name is purple-app-deployment-t4q5. The service associated with the app is currently non-operational on nodePort 32232.  
Your objective is to troubleshoot the service issue associated with the purple-app-deployment-t4q5. Ensure that the app becomes accessible and functional on nodePort 32232.  
Please proceed with the necessary diagnostics and adjustments to resolve the service issue and access the app using Purple App button on the top.  

**Solution:**
```
Analyze the service and the deployment, add ports to deployment:
---
ports:
  - containerPort: 32232
    protocol: TCP
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
8. Last week, our team successfully deployed a Nginx and PHP-FPM based setup on the Kubernetes cluster, functioning seamlessly. However, this morning, a team member made changes that resulted in disruptions, rendering the setup inoperable.  
The pod named nginx-phpfpm-t4q3 and the associated config map named nginx-config-t4q3 are experiencing issues following recent modifications. Your task is to diagnose the issue and restore functionality to the setup.  
Once the issue is resolved, copy the /home/thor/index.php file from the jump-host to the nginx-container, placing it within the document root /var/www/html. Subsequently, ensure the website becomes accessible via the Website button located in the top bar.  
Please proceed with troubleshooting the identified issues and implementing the necessary fixes. Afterward, complete the file transfer and verification of website accessibility. Also, feel free to re-create the pod if needed.  

**Solution:**  
Follow the instructions from [HERE](https://github.com/MederD/Kodekloud-Engineer-Tasks/blob/main/Tasks/Fix_Issue_with_VolumeMounts_in_Kubernetes.md)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
9. An application was previously deployed on the Kubernetes cluster, the deployment name is deployment-t5q2. This application is used by another applications within the same Kubernetes cluster. To enable access to this app, we require the creation of a ClusterIP service for the same.  
Create a service named deployment-svc-t5q2. It must be a ClusterIP service which should use port 8090 and target port should be 80.  

**Solution:**  
```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: app-t5q2
  name: deployment-svc-t5q2
spec:
  ports:
  - port: 8090
    protocol: TCP
    targetPort: 80
  selector:
    app: app-t5q2
  type: ClusterIP
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
10. One of the services named service-t5q6 needs some updates, as during deployment some required labels were missing from this service. Make changes to this service as per details mentioned below:  
Update service-t5q6 service to add a label component: front-end-t5q6.

**Solution:**  
```
kubectl edit svc service-t5q6
and add required labels
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

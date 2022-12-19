We deployed a Nginx and PHPFPM based setup on Kubernetes cluster last week and it had been working fine. This morning one of the team members made a change somewhere which caused some issues, and it stopped working. Please look into the issue and fix it:

The pod name is nginx-phpfpm and configmap name is nginx-config. Figure out the issue and fix the same.

Once issue is fixed, copy /home/thor/index.php file from jump host into nginx-container under nginx document root and you should be able to access the website using Website button on top bar.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  

First get the service and check the container port.  
Then:  
```
kubectl get pod nginx-phpfpm -o > pod.yml
```
Add:  
```
---
ports:
    - containerPort: 30080
```
and change nginx-container mounted shared volume path to /var/www/html  

delete the running pod:
```
kubectl delete pod nginx-phpfpm
```

start new pod from pod.yml:  
```
kubectl apply -f pod.yml
```

copy local file to container:  
```
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

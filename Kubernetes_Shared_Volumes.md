We are working on an application that will be deployed on multiple containers within a pod on Kubernetes cluster. There is a requirement to share a volume among the containers to save some temporary data. The Nautilus DevOps team is developing a similar template to replicate the scenario. Below you can find more details about it.   

1.) Create a pod named volume-share-devops.  

2.) For first container, use image centos with latest tag only and remember to mention tag i.e centos:latest, container should be named as volume-container-devops-1, and run a command '/bin/bash', '-c' and 'sleep 10000'. Volume volume-share should be mounted at path /tmp/ecommerce.   

3.) For second container, use image centos with latest tag only and remember to mention tag i.e centos:latest, container should be named as volume-container-devops-2, and run a command '/bin/bash', 'c' and 'sleep 10000'. Volume volume-share should be mounted at path /tmp/games.   

4.) Volumes to be named as volume-share and use emptyDir: { }.   

5.) After creating pods, exec into the first container volume-container-devops-1, and create a file ecommerce.txt with content Welcome to xFusionCorp Industries! under the mount path of first container i.e /tmp/ecommerce.  

6.) The file ecommerce.txt should be present under the mounted path /tmp/games of second container volume-container-devops-2 as they are using shared volumes.   

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.    


Solution:

Create pod.yml:   
```
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  volumes:
    - name: volume-share
      emptyDir: {}

  containers:
    - name: volume-container-devops-1
      image: centos:latest
      command: ["/bin/bash", "-c", "sleep 10000"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/ecommerce

    - name: volume-container-devops-2
      image: centos:latest
      command: ["/bin/bash", "-c", "sleep 10000"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/games
```

Exec to container "volume-container-devops-1":   
```
kubectl exec -i -t volume-share-devops --container volume-container-devops-1 -- /bin/bash
```

Create ecommerce.txt under /tmp/ecommerce, then exec to "volume-container-devops-2" and check /tmp/games

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

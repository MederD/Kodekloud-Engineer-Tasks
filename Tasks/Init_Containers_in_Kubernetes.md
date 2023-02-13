There are some applications that need to be deployed on Kubernetes cluster and these apps have some pre-requisites where some configurations need to be changed before deploying the app container. Some of these changes cannot be made inside the images so the DevOps team has come up with a solution to use init containers to perform these tasks during deployment. Below is a sample scenario that the team is going to test first.

    Create a Deployment named as ic-deploy-nautilus.

    Configure spec as replicas should be 1, labels app should be ic-nautilus, template's metadata lables app should be the same ic-nautilus.

    The initContainers should be named as ic-msg-nautilus, use image fedora, preferably with latest tag and use command '/bin/bash', '-c' and 'echo Init Done - Welcome to xFusionCorp Industries > /ic/news'. The volume mount should be named as ic-volume-nautilus and mount path should be /ic.

    Main container should be named as ic-main-nautilus, use image fedora, preferably with latest tag and use command '/bin/bash', '-c' and 'while true; do cat /ic/news; sleep 5; done'. The volume mount should be named as ic-volume-nautilus and mount path should be /ic.

    Volume to be named as ic-volume-nautilus and it should be an emptyDir type.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

Solution:  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-nautilus
  labels:
    app: ic-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-nautilus
  template:
    metadata:
      labels:
        app: ic-nautilus
    spec:
      initContainers:
      - name: ic-msg-nautilus
        image: fedora:latest
        command: ['bin/bash']
        args: ['-c', 'echo Init Done - Welcome to xFusionCorp Industries > /ic/news']        
        volumeMounts:
        - name: ic-volume-nautilus
          mountPath: '/ic'
      containers:
      - name: ic-main-nautilus
        image: fedora:latest
        command: ['bin/bash']
        args: ['-c', 'while true;do cat /ic/news;sleep 5;done']
        volumeMounts:
        - name: ic-volume-nautilus
          mountPath: '/ic'
      volumes:
      - name: ic-volume-nautilus
        emptyDir: {}
```


[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

The Nautilus DevOps team is working on to create few jobs in Kubernetes cluster. They might come up with some real scripts/commands to use, but for now they are preparing the templates and testing the jobs with dummy commands. Please create a job template as per details given below:  

1.) Create a job countdown-devops.  

2.) The spec template should be named as countdown-devops (under metadata), and the container should be named as container-countdown-devops  

3.) Use image fedora with latest tag only and remember to mention tag i.e fedora:latest, and restart policy should be Never.  

4.) Use command for i in ten nine eight seven six five four three two one ; do echo $i ; done  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.   

Solution:  

```
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-devops
spec:
  template:
    metadata:
      name: countdown-devops
    spec:
      containers:
      - name: container-countdown-devops
        image: fedora:latest
        command: ["/bin/bash", "-c", "for i in ten nine eight seven six five four three two one ; do echo $i ; done"]
      restartPolicy: Never
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

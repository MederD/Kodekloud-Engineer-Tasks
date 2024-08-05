The Nautilus DevOps team is crafting jobs in the Kubernetes cluster. While they're developing actual scripts/commands, they're currently setting up templates and testing jobs with dummy commands. Please create a job template as per details given below:


Create a job named countdown-xfusion.

The spec template should be named countdown-xfusion (under metadata), and the container should be named container-countdown-xfusion

Utilize image debian with latest tag (ensure to specify as debian:latest), and set the restart policy to Never.

Execute the command sleep 5

Note: The kubectl utility on jump_host is set up to operate with the Kubernetes cluster.

Solution:  

```
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-xfusion
spec:
  template:
    metadata:
      name: countdown-xfusion
    spec:
      containers:
      - name: container-countdown-xfusion
        image: debian:latest
        command: ["/bin/bash", "-c", "sleep 5"]
      restartPolicy: Never
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

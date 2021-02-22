There are some jobs/tasks that need to be run regularly on different schedules. Currently the Nautilus DevOps team is working on developing some scripts that will be executed on different schedules, but for the time being the team is creating some cron jobs in Kubernetes cluster with some dummy commands (which will be replaced by original scripts later). Create a cronjob as per details given below:  

1. Create a cronjob named xfusion.  
2. Set schedule to */7 * * * *.  
3. Container name should be cron-xfusion.  
4. Use httpd image with latest tag only and remember to mention tag i.e httpd:latest.  
5. Run a dummy command echo Welcome to xfusioncorp.  
6. Ensure restart policy is OnFailure.  
Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  
```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: xfusion
spec:
  schedule: "*/7 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cron-xfusion
            image: httpd:latest
            args:
            - /bin/sh
            - -c
            - echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

[Back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

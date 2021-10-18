The Nautilus DevOps team want to create a time check pod in a particular Kubernetes namespace and record the logs. This might be initially used only for testing purposes, but later can be implemented in an existing cluster. Please find more details below about the task and perform it.  

a. Create a pod called time-check in the datacenter namespace. This pod should run a container called time-check, container should use the busybox image with latest tag only and remember to mention tag i.e busybox:latest.  

b. Create a config map called time-config with the data TIME_FREQ=5 in the same namespace.  

c. The time-check container should run the command: while true; do date; sleep $TIME_FREQ;done and should write the result to the location /opt/finance/time/time-check.log. Remember you will also need to add an environmental variable TIME_FREQ in the container, which should pick value from the config map TIME_FREQ key.  

d. Create a volume log-volume and mount the same on /opt/finance/time within the container.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  
```
apiVersion: v1
kind: Namespace
metadata:
  name: datacenter

---
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-config
  namespace: datacenter
data:
    TIME_FREQ: "5"

---    
apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: datacenter
spec:
  containers:
    - name: time-check
      image: busybox:latest
      command: [ "sh", "-c", "while true; do date; sleep $TIME_FREQ;done >> /opt/finance/time/time-check.log" ]
      env:
        - name: TIME_FREQ
          valueFrom:
            configMapKeyRef:
              name: time-config
              key: TIME_FREQ
      volumeMounts:
      - name: log-volume
        mountPath: /opt/finance/time
  volumes:
    - name: log-volume
      emptyDir : {}
  restartPolicy: Never
 ```
 
 [back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

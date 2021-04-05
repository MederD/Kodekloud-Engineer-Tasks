The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:  

1.) We already created a directory /mnt/itadmin and a file index.html under the same on node01 (you need not to access node01), that location should be mounted within the container to web server's document root (keep in mind doc root can be different for Apache and Nginx web servers).  

2.) Create a PersistentVolume named as pv-devops. Configure the spec as storage class should be manual, set capacity to 7Gi, set access mode to ReadWriteOnce , volume type should be hostPath and set path to /mnt/itadmin.  

3.) Create a PersistentVolumeClaim named as pvc-devops. Configure the spec as storage class should be manual, set request storage to 5Gi, set access mode to ReadWriteOnce.  

4.) Create a pod named as pod-devops, set volume as storage-devops, and persistent volume claim to be named as pvc-devops. Container name should be container-devops, use image nginx with latest tag only and remember to mention tag i.e nginx:latest , container port should be default port 80, mount the volume to mount path to default doc root of web server and should be named storage-devops.  

5.) You can check your static website, exec into the pod and use curl command, i.e curl http://localhost.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  
Create pod-pv.yaml:  
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-devops
spec:
  capacity:
    storage: 7Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  hostPath: 
    path: "/mnt/itadmin"
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/hostname
          operator: In
          values:
            - node01

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-devops
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi

---
apiVersion: v1
kind: Pod
metadata:
  name: pod-devops
spec:
  volumes:
    - name: storage-devops
      persistentVolumeClaim:
        claimName: pvc-devops
  containers:
    - name: container-devops
      image: nginx:latest
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: storage-devops
```

Run:  
```
kubectl apply -f pod-pv.yaml
```

Run:
```
kubectl get pod pod-devops
```

Run:  
```
kubectl exec -it pod-devops -- /bin/bash
```

Run:  
```
curl http://localhost
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)


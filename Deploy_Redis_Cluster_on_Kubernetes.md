Redis is an open source in-memory data structure store, used as a database, cache and message broker. The Nautilus development team is planning to integrate Redis in one of their applications. So they want to setup a Redis cluster on Kubernetes. As per details mentioned below deploy a Redis cluster on Kubernetes.  

**1.)** Create Persistent Volumes as mentioned below:  
**a)** Create a first PersistentVolume which should be named redis-pv-01. Configure spec as accessModes which should be ReadWriteOnce, storage capacity should be 1Gi, Type should be hostPath, its hostPath should be /redis01 and directory should be already created on the worker node.  
**b.** Create a second PersistentVolume which should be named redis-pv-02. Configure spec as accessModes which should be ReadWriteOnce, storage capacity should be 1Gi, Type should be hostPath, its hostPath should be /redis02 and directory should already be created on the worker node.  
**c.** Create a third PersistentVolume which should be named redis-pv-03. Configure spec as accessModes which should be ReadWriteOnce, storage capacity should be 1Gi, Type: hostPath, hostPath: /redis03, directory should be already created on the worker node.  
**d.** Creata a fourth PersistentVolume which should be named redis-pv-04. Configure spec as accessModes should be ReadWriteOnce, storage capcity should be 1Gi, Type should be hostPath, its hostPath should be /redis04 and directory should be already created on the worker node.  
**e.** Create a fifth PersistentVolume which should be named as redis-pv-05. Configure spec as accessModes which should be ReadWriteOnce, storage capacity should be 1Gi, Type should be hostPath, its hostPath should be /redis05 and directory should be already created on the worker node.  
**f.** Create a sixth PersistentVolume which should be named redis-pv-06. Configure spec as accessModes which should be ReadWriteOnce, storage capacity should be 1Gi, Type should be hostPath, hostPath should be /redis06 and directory should be already created on the worker node. 
 
**2.)** We already created ConfigMap named as redis-cluster-configmap. You can try to inspect it.  

**3.)** Create a service which should be named redis-cluster-service. Configure spec as first port name should be client, its port should be 6379 and its targetPort should be 6379. The second port name should be gossip, its port should be 16379 and its targetPort should be 16379, its type should be ClusterIP. And the selector's app should be redis-cluster.  

**4.)** Create a StatefulSet which should be named redis-cluster. Confiugre spec as replicas should be 6, selector's matchLabels app should be redis-cluster. Template's labels app should be redis-cluster under the metadata. The container name should be redis-container, use image redis:5.0.1-alpine ( use exact name of image as mentioned ), use command: ["/conf/update-node.sh", "redis-server", "/conf/redis.conf"], env name should be POD_IP, valueFrom should be fieldRef, fieldPath should be status.podIP (apiVersion: v1). First port name should be client, its containerPort should be 6379, second port name should be gossip, its containerPort should be 16379. First volumeMount name should be conf, its mountPath should be /conf and readOnly should be false ( ConfigMap Mount ), second volumeMount name should be data, its mountPath should be /data, readOnly should be false ( volumeClaim ). Volume name should be conf, its configMap name should be redis-cluster-configmap and its defaultMode should be 0755. volumeClaimTemplates name should be data under metadata, accessModes should be ReadWriteOnce and storage request should be 1Gi.  

**5.)** Configure the Cluster. Once the StatefulSet has been deployed with 6 Running pods, run the following commands and type yes when prompted. Command: kubectl exec -it redis-cluster-0 -- redis-cli --cluster create --cluster-replicas 1 $(kubectl get pods -l app=redis-cluster -o jsonpath='{range.items[*]}{.status.podIP}:6379 {end}').  
**Note:** The kubectl on jump_host has been configured to work with the kubernetes cluster.  

**Solution:**  

**1.)** Start with creating Persistent Volumes:  
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: redis-pv-01
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/redis01"
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: redis-pv-02
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/redis02"
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: redis-pv-03
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/redis03"
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: redis-pv-04
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/redis04"
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: redis-pv-05
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/redis05"
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: redis-pv-06
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/redis06"
```

**2.)** Inspect "redis-cluster-configmap"  
```
kubectl describe configmap redis-cluster-configmap
```  

**3.)** Create service:  
```
apiVersion: v1
kind: Service
metadata:
  name: redis-cluster-service
spec:
  type: ClusterIP
  selector:
    app: redis-cluster
  ports:
    - name: client
      port: 6379
      targetPort: 6379
    - name: gossip
      port: 16379
      targetPort: 16379
```  

**4.)** Create StatefulSet:  
```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: redis-cluster
spec:
  serviceName: redis-cluster-service
  replicas: 6
  selector:
    matchLabels:
      app: redis-cluster
  template:
    metadata:
      labels:
        app: redis-cluster
    spec:
      containers:
      - name: redis-container
        image: redis:5.0.1-alpine
        command: ["/conf/update-node.sh", "redis-server", "/conf/redis.conf"]
        ports:
        - name: client
          containerPort: 6379
        - name: gossip
          containerPort: 16379
        env:
        - name: POD_IP
          valueFrom:
            fieldRef: 
              fieldPath: status.podIP
        volumeMounts:
        - name: conf
          mountPath: /conf
          readOnly: false
        - name: data
          mountPath: /data
          readOnly: false
      volumes:
        - name: conf
          configMap: 
            name: redis-cluster-configmap
            defaultMode: 0755     
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

**5.)** Run following command, once StatefulSet has been deployed with 6 running pods:  
```
Command: kubectl exec -it redis-cluster-0 -- redis-cli --cluster create --cluster-replicas 1 $(kubectl get pods -l app=redis-cluster -o jsonpath='{range.items[*]}{.status.podIP}:6379 {end}')
```
... type yes when prompted.  

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

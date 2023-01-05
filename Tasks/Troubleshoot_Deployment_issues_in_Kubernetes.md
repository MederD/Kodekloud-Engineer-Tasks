Last week, the Nautilus DevOps team deployed a redis app on Kubernetes cluster, which was working fine so far. This morning one of the team members was making some changes in this existing setup, but he made some mistakes and the app went down. We need to fix this as soon as possible. Please take a look.

The deployment name is redis-deployment. The pods are not in running state right now, so please look into the issue and fix the same.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

Solution:  
Check configmap and deployment:
```
kubectl get configmap
```
```
kubectl describe deploy
```
```
kubectl logs <pod-name>
```

In this case there were typos in configmap volume mount and image name.
We have to fix the typos:
```
kubectl edit deploy
```
Fix "alpin"to "alpine" and "cofig" to "config".

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

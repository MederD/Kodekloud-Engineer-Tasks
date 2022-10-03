The Nautilus DevOps team is working to deploy some tools in Kubernetes cluster. Some of the tools are licence based so that licence information needs to be stored securely within Kubernetes cluster. Therefore, the team wants to utilize Kubernetes secrets to store those secrets. Below you can find more details about the requirements:

    We already have a secret key file beta.txt under /opt location on jump host. Create a generic secret named beta, it should contain the password/license-number present in beta.txt file.

    Also create a pod named secret-devops.

    Configure pod's spec as container name should be secret-container-devops, image should be ubuntu preferably with latest tag (remember to mention the tag with image). Use sleep command for container so that it remains in running state. Consume the created secret and mount it under /opt/demo within the container.

    To verify you can exec into the container secret-container-devops, to check the secret key under the mounted path /opt/demo. Before hitting the Check button please make sure pod/pods are in running state, also validation can take some time to complete so keep patience.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

Solution:
```
kubectl create secret generic beta \
--from-file=/opt/beta.txt
```
```
apiVersion: v1
kind: Pod
metadata:
  name: secret-devops
spec:
  containers:
    - name: secret-container-devops
      image: ubuntu:latest
      command: [ "/bin/bash", "-c", "--" ]
      args: ["while true; do sleep 30; done;"]
      volumeMounts:
      - name: secret
        mountPath: /opt/demo
  volumes:
  - name: secret
    secret:
      secretName: beta
```
```
kubectl exec -it secret-devops -- bin/bash
```
```
cat /opt/demo/beta.txt
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 


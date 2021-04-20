This morning the Nautilus DevOps team rolled out a new release for one of the applications. Recently on of the customers logged a complaint which seems to be about a bug related to the recent release. Therefore, the team wants to rollback the recent release.  

There is a deployment named nginx-deployment; roll it back to the previous revision.  

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.  

Solution:  

```
kubectl rollout undo deployment nginx-deployment
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

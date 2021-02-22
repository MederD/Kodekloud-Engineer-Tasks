Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on Application Server 1. Please complete the task as per details given below:   

1.) On Application Server 1 create a container named nginx_1 using image nginx with alpine tag and make sure container is in running state.   


Solution:   

SSH to stapp01 and:   
```
sudo docker run -d --name nginx_1 nginx:alpine
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

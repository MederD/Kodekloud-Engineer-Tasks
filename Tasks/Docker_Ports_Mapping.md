The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on Application Server 2 in Stratos Datacenter. Please perform the task as per details mentioned below:
a. Pull nginx:alpine docker image on Application Server 2.
b. Create a container named blog using the image you pulled.
c. Map host port 5001 to container port 80. Please keep the container in running state.

Solution:

1. SSH to stapp02 and pull the image:  
```
sudo docker pull nginx:alpine
```

2. Create container with name "blog" and map the ports:  
```
sudo docker run -itd --name blog -p 5001:80 nginx:alpine
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

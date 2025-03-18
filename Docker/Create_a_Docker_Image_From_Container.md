One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:  
a. Create an image cluster:datacenter on Application Server 2 from a container ubuntu_latest that is running on same server.  

Solution:  
```bash
ssh steve@stapp02
sudo docker commit ubuntu_latest cluster:datacenter
sudo docker image ls
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

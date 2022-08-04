The Nautilus DevOps team has some conditional data present on App Server 3 in Stratos Datacenter. There is a container ubuntu_latest running on the same server. We received a request to copy some of the data from the docker host to the container. Below are more details about the task:

On App Server 3 in Stratos Datacenter copy an encrypted file /tmp/nautilus.txt.gpg from docker host to ubuntu_latest container (running on same server) in /opt/ location. Please do not try to modify this file in any way.

Solution:  
ssh to banner@stapp03 and run:  
```
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt  
```   

To check if file was copied, you can run:  
```
docker exec -it ubuntu_latest /bin/bash   
ls /opt
```


[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)
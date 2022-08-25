The Nautilus DevOps team is testing applications containerization, which issupposed to be migrated on docker container-based environments soon. In today's stand-up meeting one of the team members has been assigned a task to create and test a docker container with certain requirements. Below are more details:

a. On App Server 3 in Stratos DC pull nginx image (preferably latest tag but others should work too).

b. Create a new container with name demo from the image you just pulled.

c. Map the host volume /opt/itadmin with container volume /home. There is an sample.txt file present on same server under /tmp; copy that file to /opt/itadmin. Also please keep the container in running state.

Solution:
```
ssh banner@stapp03
sudo docker pull nginx:latest
sudo cp /tmp/sample.txt /opt/itadmin
sudo docker run -td -v /opt/itadmin:/home --name demo nginx:latest
sudo docker container inspect demo
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

The Nautilus DevOps team is testing applications containerization, which is supposed to be migrated on docker container-based environments soon.  
In today's stand-up meeting one of the team members has been assigned a task to create and test a docker container with certain requirements. Below are more details:  
a. On App Server  1 in Stratos DC pull nginx image (preferably latest tag but others should work too).  
b. Create a new container with name official from the image you just pulled.  
c. Map the host volume /opt/finance with container volume /home. There is an sample.txt file present on same server under /tmp; copy that file to /opt/finance. Also please keep the container in running state.  

Solution:  
1. SSH to the app server requested - it may be a different app server for you.

    ```bash
    ssh tony@stapp01
    ```

2. Become root to save typing `sudo` on every command

    ```bash
    sudo su
    ```

3. Pull the image

    ```bash
    docker pull nginx:latest
    ```

4.  Create the container

    ```bash
    docker run -it -d -v /opt/finance:/home --name official nginx:latest
    ```

6. Copy the file
   ```bash
   cp /tmp/sample.txt /opt/finance/sample.txt
   ```
[reference](https://docs.docker.com/engine/storage/volumes/)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

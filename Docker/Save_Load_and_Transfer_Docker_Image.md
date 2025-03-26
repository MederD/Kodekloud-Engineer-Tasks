One of the DevOps team members was working on to create a new custom docker image on App Server 1 in Stratos DC. He is done with his changes and image is saved on same server with name media:datacenter.  
Recently a requirement has been raised by a team to use that image for testing, but the team wants to test the same on App Server 3. So we need to provide them that image on App Server 3 in Stratos DC.  
a. On App Server 1 save the image media:datacenter in an archive.  
b. Transfer the image archive to App Server 3.  
c. Load that image archive on App Server 3 with same name and tag which was used on App Server 1.  
Note: Docker is already installed on both servers; however, if its service is down please make sure to start it.  

Solution:  
1. SSH to the app server requested - it may be a different app server for you.

    ```bash
    ssh tony@stapp01
    ```

2. Become root to save typing `sudo` on every command

    ```bash
    sudo su
    ```

3. Archive the image 

    ```bash
    docker save media:datacenter > /tmp/media-image.tar
    ```

4.  Copy over to App Server 3

    ```bash
    scp /tmp/media-image.tar banner@stapp03:/tmp/media-image.tar
    ```

6. SSH to App Server 3 and load
   ```bash
   ssh banner@stapp03
   sudo docker load < /tmp/media-image.tar
   sudo docker ls
   ```
[reference](https://gcore.com/learning/how-to-transfer-move-a-docker-image-to-another-system/)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

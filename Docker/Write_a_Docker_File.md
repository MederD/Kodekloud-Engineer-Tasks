As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects.  
Several of the initial testing requirements are already been shared with DevOps team.  
Therefore, create a docker file /opt/docker/Dockerfile (please keep D capital of Dockerfile) on App server 3 in Stratos DC and configure to build an image with the following requirements:  
a. Use ubuntu as the base image.  
b. Install apache2 and configure it to work on 5002 port. (do not update any other Apache configuration settings like document root etc).  

Solution:  
1. SSH to the app server requested - it may be a different app server for you.

    ```bash
    ssh banner@stapp03
    ```

2. Become root to save typing `sudo` on every command

    ```bash
    sudo su
    ```

3. Creata Dockerfile `/opt/docker/Dockerfile`

    ```bash
    FROM ubuntu:latest
    RUN apt-get update -y
    RUN apt-get install apache2 -y
    RUN sed -i 's/^Listen 80$/Listen 5002/' /etc/apache2/ports.conf
    RUN sed -i 's/^<VirtualHost \*:80>$/<VirtualHost *:5002>/' /etc/apache2/sites-available/*.conf
    EXPOSE 5002
    CMD ["apachectl", "-D", "FOREGROUND"]
    ```

4.  Build and run the image

    ```bash
    docker build -t apache_image:1.0 .
    docker image ls
    docker run --name myapache -d -p 5002:5002 apache_image:1.0
    docker ps
    ```

6. Exec and check the apache
   ```bash
   docker exec -it myapache /bin/bash
   curl http://localhost:5002
   ```
[reference](https://www.digitalocean.com/community/tutorials/apache-web-server-dockerfile)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)



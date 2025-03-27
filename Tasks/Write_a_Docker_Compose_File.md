The Nautilus application development team shared static website content that needs to be hosted on the httpd web server using a containerized platform. The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines. Below are the details:  

a. On App Server 3 in Stratos DC create a container named httpd using a docker compose file /opt/docker/docker-compose.yml (please use the exact name for file).  

b. Use httpd (preferably latest tag) image for container and make sure container is named as httpd; you can use any name for service.  

c. Map 80 number port of container with port 6100 of docker host.  

d. Map container's /usr/local/apache2/htdocs volume with /opt/data volume of docker host which is already there. (please do not modify any data within these locations).  

Solution:  
```
version: '3'
services:
    httpd:
      volumes:
        - /opt/data:/usr/local/apache2/htdocs
      image: httpd:latest
      container_name: httpd
      ports:
        - 6100:80
```
[reference](https://docs.docker.com/reference/cli/docker/compose/)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

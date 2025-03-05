1. The Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on Application Server 1. Please complete the task as per details given below:  
On Application Server 1 create a container named nginx_1 using image nginx with alpine tag and make sure container is in running state.  
**Solution:**
```bash
docker run -d --name nginx_1 nginx:alpine
```
---
2. The Nautilus team wants to create a debug container on Application Server 1. However, they had some specific requirements related to the CMD. Please complete the task as per details given below:  
a. On Application Server 1 create a container named debug_1 using image ubuntu/apache2:latest.  
b. Overwrite the default CMD with command sleep 1000.  
c. Make sure the container is in running state.  
**Solution:**  
```bash
docker run -d --name debug_1 ubuntu/apache2:latest sleep 1000
```
---
3. We received a request to copy some of the data from one of the docker containers to the docker host. The container is running on App Server 1 in Stratos Datacenter. Below are more details about the task:  
On App Server 1 in Stratos Datacenter copy an encrypted file /tmp/test.txt.gpg from development_3 docker container to the docker host in /tmp location. Please do not try to modify this file in any way.  
**Solution:**
```bash
docker cp development_3:/tmp/test.txt.gpg /tmp/test.txt.gpg
```
---
4. The Nautilus DevOps team has some confidential data present on App Server 1 in Stratos Datacenter. There is a container ubuntu_latest running on the same server. We received a request to copy some of the data from the docker host to the container.
   Below are more details about the task:  
On App Server 1 in Stratos Datacenter copy an encrypted file /tmp/nautilus.txt.gpg from docker host to ubuntu_latest container (running on same server) in /home/ location (create this location if doesn't exit). Please do not try to modify this file in any way.  
**Solution:**
```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/home/nautilus.txt.gpg
```
---
5. There is a docker image on Application Server 1 in Stratos DC. This image needs to be retagged as per details given below:  
Re-tag the nginx:mainline-alpine3.18-slim image as nginx:nautilus  
**Solution:**
```bash
docker tag nginx:mainline-alpine3.18-slim nginx:nautilus
```
---
6. The Nautilus DevOps team wanted to transfer some docker images from one server to another server so they wanted to save some docker images as tar archives on the docker host itself. Find below the exact requirements:  
There is a docker image named nginx:mainline-alpine-slim on App Server 1 in Stratos Datacenter, save this image as nginx.tar archive under /home directory on the same server.  
**Solution:**
```bash
docker save -o /home/nginx.tar nginx:mainline-alpine-slim
```
---
7. The Nautilus DevOps team is planning to setup/create some docker containers on App Server 1 in Stratos Datacenter, some prerequisites are needs to be done on this server. Find below more details:  
Create a new network named mysql-network using the bridge driver. Allocate subnet 182.18.0.0/24, configure Gateway 182.18.0.1.  
**Solution:**
```bash
  docker network create --driver bridge --subnet 182.18.0.0/24 --gateway 182.18.0.1 mysql-network
```
---
8. The Nautilus DevOps team is planning to do some cleanup on App Server 1 in Stratos Datacenter, some old and unused docker networks need to be deleted. Find below more details:  
Delete a docker network named php-network from App Server 1 in Stratos Datacenter.  
**Solution:**
```bash
docker network rm php-network
```
---
9. There is a static website running within a container named nautilus, this container is running on App Server 1. Suddenly, we started facing some issues with the static website on App Server 1.
    Look into the issue to fix the same, you can find more details below:  
a. Container's volume /usr/local/apache2/htdocs is mapped with the host volume /var/www/html.  
b. The website should run on host port 8080 on App Server 1 i.e command curl http://localhost:8080/ should work on App Server 1.  
**Solution:**
```bash
docker start nautilus
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)


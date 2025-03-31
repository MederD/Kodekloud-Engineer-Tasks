The Nautilus DevOps team is working to deploy one of the applications on App Server 3 in Stratos DC. Due to a misconfiguration in the docker compose file, the deployment is failing. We would like you to take a look into it to identify and fix the issues. More details can be found below:  

a. docker-compose.yml file is present on App Server 3 under /opt/docker directory.  

b. Try to run the same and make sure it works fine.  

c. Please do not change the container names being used. Also, do not update or alter any other valid config settings in the compose file or any other relevant data that can cause app failure.   

Note: Please note that once you click on FINISH button all existing running/stopped containers will be destroyed, and your compose will be run.   

Solution:  
docker-compose.yml file should be as below:  
```
version: "2.2"
services:
    web:
        build: ./app
        container_name: python
        ports:
            - "5000:5000"
        volumes:
            - ./app:/code
        depends_on:
            - redis
    redis:
        image: redis
        container_name: redis
```

run below command:  
```
sudo docker-compose up -d
```

check the running containers:  
```
sudo docker ps -a
```

curl localhost:  
```
curl http://localhost:5000
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

 





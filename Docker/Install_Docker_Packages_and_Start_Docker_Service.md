The Nautilus DevOps team aims to containerize various applications following a recent meeting with the application development team. They intend to conduct testing with the following steps:  
    Install docker-ce and docker compose packages on App Server 2.  
    Initiate the docker service.  
Solution:  
```
ssh steve@stapp02
sudo dnf update -y
sudo dnf install -y yum-utils device-mapper-persistent-data lvm2
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
sudo dnf install -y docker-ce --nobest
sudo systemctl enable --now docker
sudo systemctl status docker
sudo docker --version
sudo dnf install docker-compose-plugin
docker compose version
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)   
[reference 1](https://tecadmin.net/how-to-install-docker-on-centos-stream-9/)  
[reference 2](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-rocky-linux-9)

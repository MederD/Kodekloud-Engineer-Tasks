The Nautilus DevOps team possesses confidential data on App Server 3 in the Stratos Datacenter. A container named ubuntu_latest is running on the same server.  
Copy an encrypted file /tmp/nautilus.txt.gpg from the docker host to the ubuntu_latest container located at /opt/. Ensure the file is not modified during this operation.  

Solution:  
```
ssh banner@stapp03
sudo docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt/
sudo docker exec -it ubuntu_latest  ls /opt/
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

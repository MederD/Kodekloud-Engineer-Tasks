One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 2 in Stratos Datacenter. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:  

a. Install apache2 in kkloud container using apt that is running on App Server 2 in Stratos Datacenter.  

b. Configure Apache to listen on port 6300 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.  

c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.  

Solution:  
```
ssh steve@stapp02  
sudo su  
docker ps -a  
docker exec -it kkloud /bin/bash  
apt update -y  
apt install apache2 -y  
cd /etc/apache2  
vi ports.conf  
change port 80 to 6300 in ports.conf
vi apache2.conf
if there's 80 change it to 6300
change ServerName to lcoalhost  
service apache2 start  
service apache2 enable  
service apache2 status  
exit 
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

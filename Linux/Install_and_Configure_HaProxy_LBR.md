There is a static website running in Stratos Datacenter. They have already configured the app servers and code is already deployed there.  
To make it work properly, they need to configure LBR server. There are number of options for that, but team has decided to go with HAproxy.  
FYI, apache is running on port 8085 on all app servers. Complete this task as per below details.
a. Install and configure HAproxy on LBR server using yum only and make sure all app servers are added to HAproxy load balancer.  
HAproxy must serve on default http port (Note: Please do not remove stats socket /var/lib/haproxy/stats entry from haproxy default config.).
b. Once done, you can access the website using StaticApp button on the top bar.  

Solution:  
```
ssh loki@stlb01
sudo yum update -y
sudo yum install haproxy -y
sudo vi /etc/haproxy/haproxy.cfg
```
change:  
```
frontend main
  bind *:80
---

backend app
  balance     roundrobin
  server  stapp01 172.16.238.10:8085 check
  server  stapp02 172.16.238.11:8085 check
  server  stapp03 172.16.238.12:8085 check
```
```
haproxy -f /etc/haproxy/haproxy.cfg
sudo systemctl start haproxy
sudo systemctl status haproxy
curl stapp01:8085
curl stapp02:8085
curl stapp03:8085
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 


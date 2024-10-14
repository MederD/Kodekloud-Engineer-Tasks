Some users of the monitoring app have reported issues with xFusionCorp Industries mail server.  
They have a mail server in Stork DC where they are using postfix mail transfer agent. Postfix service seems to fail. Try to identify the root cause and fix it.  

Solution:  
```
ssh groot@stmail01
sudo su
```
Uncomment/Edit/Add below configuration on /etc/postfix/main.conf:  
```
myhostname = stmail01.stratos.xfusioncorp.com	
mydomain = stratos.xfusioncorp.com	
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
mynetworks = 172.16.238.0/24, 127.0.0.0/8
# inet_interfaces = localhost
```
```
systemctl start postfix
systemctl status postfix
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

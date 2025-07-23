We have one of our websites up and running on our Nautilus infrastructure in Stratos DC.  
Our security team has raised a concern that right now Apache's port i.e 8086 is open for all since there is no firewall installed on these hosts.  
So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:  
1. Install iptables and all its dependencies on each app host.
2. Block incoming port 8086 on all apps for everyone except for LBR host.
3. Make sure the rules remain, even after system reboot.

Solution:  
```
ssh tony@stapp01
sudo su
yum install iptables-services -y
systemctl start iptables
systemctl enable iptables
systemctl status iptables
iptables -I INPUT 1 -p tcp --dport 8086 -s 172.16.238.14 -j ACCEPT
iptables -A INPUT -p tcp --dport 8086 -j DROP
iptables -L -n --line-numbers
iptables-save > /etc/sysconfig/iptables
```
check from stlb01 if you able to connect:  
```
ssh loki@stlb01
telnet stapp01 8086
```
[reference](https://www.redhat.com/en/blog/iptables)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 8085 (which is the Apache port).  
The service itself could be down, the firewall could be at fault, or something else could be causing the issue.  
Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.  
Once fixed, you can test the same using command curl http://stapp01:8085 command from jump host.  

Solution:
```
ssh tony@stapp01
sudo su
netstat -tulpn | grep 8085
kill -9 470 (in my case 470 sendmail)
systemctl restart httpd
systemctl status httpd
sudo iptables -I INPUT -p tcp -m tcp --dport 8085 -j ACCEPT  && sudo iptables-save > /etc/sysconfig/iptables &&  cat /etc/sysconfig/iptables
```  
[reference](https://www.redhat.com/en/blog/iptables)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

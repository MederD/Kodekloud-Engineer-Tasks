xFusionCorp Industries has an application running on Nautlitus infrastructure in Stratos Datacenter.  
The monitoring tool recognised that there is an issue with the haproxy service on LBR server. That needs to fixed to make the application work properly.  
Troubleshoot and fix the issue, and make sure haproxy service is running on Nautilus LBR server. Once fixed, make sure you are able to access the website using StaticApp button on the top bar.  

Solution:  
```
ssh loki@stlb01
sudo systemctl status haproxy
sudo haproxy -f /etc/haproxy/haproxy.cfg
```
Last command will return the configuration errors, like:  
```
[ALERT] 294/180234 (501) : parsing [/etc/haproxy/haproxy.cfg:50] : balance only supports 'roundrobin', 'static-rr', 'leastconn', 'source', 'uri', 'url_param', 'hdr(name)' and 'rdp-cookie(name)' options.
[ALERT] 294/180234 (501) : parsing [/etc/haproxy/haproxy.cfg:57] : balance only supports 'roundrobin', 'static-rr', 'leastconn', 'source', 'uri', 'url_param', 'hdr(name)' and 'rdp-cookie(name)' options.
[ALERT] 294/180234 (501) : Error(s) found in configuration file : /etc/haproxy/haproxy.cfg
[ALERT] 294/180234 (501) : Fatal errors found in configuration.
```
from above we can see that there's an error related to a loadbalancing algorithm. Fix where it says:  
```
balance round
```
to:  
```
balance roundrobin
```
then:  
```
sudo systemctl restart haproxy
sudo systemctl status haproxy
```  

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 


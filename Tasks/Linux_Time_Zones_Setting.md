During the daily standup, it was pointed out that the timezone across Nautilus Application Servers in Stratos Datacenter doesn't match with that of the local datacenter's timezone, which is Pacific/Chatham.

Solution:  

SSH to app servers, run: 
```
timedatectl
```
this will show what the current timezone settings, then run:  
```
sudo timedatectl set-timezone Pacific/Chatham
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

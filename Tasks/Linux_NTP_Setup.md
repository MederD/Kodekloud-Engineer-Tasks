The system admin team of xFusionCorp Industries has noticed an issue with some servers in Stratos Datacenter where some of the servers are not in sync w.r.t time. Because of this, several application functionalities have been impacted. To fix this issue the team has started using common/standard NTP servers. They are finished with most of the servers except App Server 3. Therefore, perform the following tasks on this server:

1. Install and configure NTP server on App Server 3.
2. Add NTP server 2.north-america.pool.ntp.org in NTP configuration on App Server 3.
3. Please do not try to start/restart/stop ntp service, as we already have a restart for this service scheduled for tonight and we don't want these changes to be applied right now.

Solution:  
- SSH to App Server 3
- Install ntpd:
  ```
  yum install -y ntp
  ```
- Add NTP server to /etc/ntp.conf

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

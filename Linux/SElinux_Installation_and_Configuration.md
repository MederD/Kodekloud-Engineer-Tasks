Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App server 1 in the Stratos Datacenter:  
    Install the required SELinux packages.  
    Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.  
    No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.  
    Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.  

Solution:  
```
ssh tony@stapp01
sudo rpm -aq | grep selinux
sudo dnf install -y selinux-policy-targeted policycoreutils policycoreutils-python-utils
sudo vi /etc/selinux/config
```
change `SELINUX=enfrcing` to `SELINUX=disabled`  


[reference](https://www.linode.com/docs/guides/a-beginners-guide-to-selinux-on-centos-7/)    
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

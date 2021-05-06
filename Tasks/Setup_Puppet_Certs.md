**The Nautilus DevOps team has set up a puppet master and an agent node in Stratos Datacenter. Puppet master is running on jump host itself (also note that Puppet master node is also running as Puppet CA server) and Puppet agent is running on App Server 1. Since it is a fresh set up, the team wants to sign certificates for puppet master as well as puppet agent nodes so that they can proceed with the next steps of set up. You can find more details about the task below:**  

**Puppet server and agent nodes are already have required packages, but you may need to start puppetserver (on master) and puppet service on both nodes.**  

**1.) Assign and sign certificates for both master and agent node.**  

**Solution:**  

On jump_host and stapp01 add word "puppet" at the end of the hostnames.  
```
vi /etc/hosts
```
Then, restart the services on master node:  
```
systemctl restart puppetserver
systemctl start puppet
systemctl status puppetserver
systemctl status puppet
```

and on stapp01:  
```
sudo systemctl start puppet
sudo systemctl status puppet
```

On stapp01:  
```
puppet agent -t
```

On master node:  
```
puppetserver ca list --all
puppetserver ca sign --all
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  


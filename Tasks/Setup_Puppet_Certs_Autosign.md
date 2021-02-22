During the last weekly meeting, the Nautilus DevOps team decided to use Puppet autosign config to auto sign certificates for all Puppet agent nodes they will keep adding under Puppet master in Stratos DC. Puppet master and CA servers are currently running on jump host and they have configured all three app servers as Puppet agents. To set up autosign configuration on the Puppet master server, some configuration settings must be set up. Please find more details below:  

The Puppet server package is already installed on puppet master i.e jump server and the Puppet agent package is already installed on all App Servers. However, you may need to begin required services on all these servers.  

1.) Configure autosign configuration on the Puppet master i.e jump server (by creating autosign.conf in puppet configuration directory) and assign certificates for both master node as well as for all agent nodes. Use host's FDQN to assign the certificates.  

2.) Use alias puppet (dns_alt_names) for master node and add its entry in /etc/hosts config file on master i.e Jump Server as well as on all agent nodes i.e App Servers.  

Note: Before submitting your task please verify if all certificates have been generated, sometimes it takes time to sign certificates.  

Solution:  
1.) Create autosign.conf file in /etc/puppetlabs/puppet and add below lines:  
```
jump_host.stratos.xfusioncorp.com
stapp01.stratos.xfusioncorp.com
stapp02.stratos.xfusioncorp.com
stapp03.stratos.xfusioncorp.com
```

2.) Configure /etc/puppetlabs/puppet/puppet.conf:   
```
[master]
dns_alt_names = jump_host.stratos.xfusioncorp.com,puppet

[main]
certname = jump_host.stratos.xfusioncorp.com
server = puppet
```

3.) Configure /etc/hosts by adding "puppet":  
```
...
172.16.238.10   stapp01.stratos.xfusioncorp.com
172.16.238.11   stapp02.stratos.xfusioncorp.com
172.16.238.12   stapp03.stratos.xfusioncorp.com
172.16.239.2    jump_host.stratos.xfusioncorp.com jump_host puppet
172.16.238.3    jump_host.stratos.xfusioncorp.com jump_host puppet
```

4.) Restart puppet service:  
```
Systemctl restart puppet
```

5.) SSH to stapp01, stapp02 and stapp03 and configure /etc/puppetlabs/puppet/puppet.conf, add below lines:   
```
certname = stapp01.stratos.xfusioncorp.com
server = puppet
```

6.) Configure /etc/hosts on stapp01, stapp02 and stapp03 by adding "puppet":  
```
172.16.239.2    jump_host.stratos.xfusioncorp.com jump_host puppet
172.16.238.3    jump_host.stratos.xfusioncorp.com jump_host puppet
172.16.238.10   stapp01.stratos.xfusioncorp.com
```

7.) Restart puppet service on App servers:  
```
sudo systemctl restart puppet
```

8.) Run below command on App servers:  
```
puppet agent -t
```

9.) On jump_host check the signed certificates:  
```
ls /etc/puppetlabs/puppet/ssl/ca/signed
```

or
```
puppetserver ca list --all
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)   



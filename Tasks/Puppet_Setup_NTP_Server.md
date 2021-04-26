While troubleshooting one of the issue on app servers in Stratos Datacenter DevOps team identified the root cause that the time isn't synchronized properly among all app servers which cause issues sometimes. So team has decided to use a specific time server for all app servers so that they all remain in sync. This task needs to be done using Puppet so as per details mentioned below please compete the task:    

1.) Create a puppet programming file ecommerce.pp under /etc/puppetlabs/code/environments/production/manifests directory on puppet master node i.e on Jump Server. Within the programming file define a custom class ntpconfig to install and configure ntp server on all app servers.    

2.) Also add NTP Server server 2.oceania.pool.ntp.org in default configuration file on all app servers.    

3.) Please note that do not try to start/restart/stop ntp service as we already have a scheduled restart for this service tonight and we don't want these changes to be applied right now.   

Note: Please perform this task using ecommerce.pp only, do not try to create any separate inventory file.   

Solution:

1.) Run below command on jump_host:  
```
puppet module install puppetlabs-ntp
```

2.) Create ecommerce.pp:  
```
class { 'ntp':
servers => [ '2.oceania.pool.ntp.org' ],
}
class ntpconfig {
include ntp
}

node 'jump_host.stratos.xfusioncorp.com','stapp01.stratos.xfusioncorp.com','stapp02.stratos.xfusioncorp.com','stapp03.stratos.xfusioncorp.com' {
include ntpconfig
}
```

3.) Run on jump_host:  
```
puppet apply ecommerce.pp
```

4.) SSH to all app servers and run:  
```
sudo puppet agent -t
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

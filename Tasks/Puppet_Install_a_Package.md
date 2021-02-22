New packages need to be installed on all app servers in Stratos Datacenter. The Nautilus DevOps team has decided to install the same using Puppet. Since jump host is already configured to run as Puppet master server and all app servers are already configured to work as puppet agent nodes, we need to create required manifests on the Puppet master server so that the same can be applied on all Puppet agent nodes. Please find more details about the task below.  

Create a Puppet programming file blog.pp under /etc/puppetlabs/code/environments/production/manifests directory on master node i.e Jump Server and using puppet package resource perform the tasks below.  

1.) Install package nginx through Puppet package resource on all Puppet agent nodes i.e all App Servers.  

Note: Please perform this task using blog.pp only, do not create any separate inventory file.  

Solution:

Create blog.pp under /etc/puppetlabs/code/environments/production/manifests:
```
  package {'nginx':
    ensure => 'installed',
  }

```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

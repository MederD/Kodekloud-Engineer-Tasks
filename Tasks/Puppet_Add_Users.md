As a new teammate has joined Nautilus application development team, the application development team has asked the DevOps team to create a new user for the new teammate on all application servers in Stratos Datacenter. The task needs to be performed using Puppet only. You can find more details below about the task.  

Create a Puppet programming file games.pp under /etc/puppetlabs/code/environments/production/manifests directory on master node i.e Jump Server, and using Puppet user resource add a user on all app servers as mentioned below:  

1.) Create a user siva and set its UID to 1203 on all Puppet agent nodes i.e all App Servers.  

Note: Please perform this task using games.pp only, do not create any separate inventory file.  

Solution:  
```
node 'stapp01.stratos.xfusioncorp.com','stapp02.stratos.xfusioncorp.com','stapp03.stratos.xfusioncorp.com' {
include user
}

class user {
   user { 'siva':
      ensure => present,
      uid => '1203'
   }
}

```

SSH to app servers, and run:  
```
sudo puppet agent -t
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

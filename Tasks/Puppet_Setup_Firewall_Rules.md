The Nautilus DevOps team was auditing some of the applications running on all app servers in Stratos Datacenter. They found some security loopholes-for example, they observed that there is no firewall installed on these apps. So, the team has decided to install firewalld on all App Servers. Some rules need to be added. This task needs to be done using Puppet; please complete the task per the following details:  

Create an inventory file code.pp under /etc/puppetlabs/code/environments/production/manifests directory on Puppet master node i.e on Jump Server. In this inventory file you need to define node specific classes only which are mentioned below.  

Define a class firewall_node1 for agent node 1 i.e App Server 1, firewall_node2 for agent node 2 i.e App Server 2, firewall_node3 and for agent node3 i.e App Server 3.
Also create a Puppet programming file apps.pp under /etc/puppetlabs/code/environments/production/manifests directory on Puppet master node i.e on Jump Server and write code to perform the following task.  

Install puppet firewall module on master node i.e on Jump Server (you can install manually).  

There are some different applications running on all three apps. One of the applications is using port 8085 on App server 1 , 9008 on App server 2 and 8097 on App server 3. Complete below mentioned tasks:  

a. Open all incoming connections for 8085/tcp port on App Server 1 and zone should be public.  

b. Open all incoming connections for 9008/tcp port on App Server 2 and zone should be public.  

c. Open all incoming connections for 8097/tcp port on App Server 3 and zone should be public.  

Note: Please do not add firewall rich rules.  


Solution:    

Install puppet-firewalld on jump_host:  
```
puppet module install puppet-firewalld --version 4.4.0
```

After that, run:  
```
service puppet start
```

Go to /etc/puppetlabs/code/environments/production/manifests and create apps.pp:  
```
class { 'firewalld': }

class firewall_node1 {
  firewalld_port { 'Open port 8085 for App Server 1':
    ensure   => present,
    zone     => 'public',
    port     => 8085,
    protocol => 'tcp',
  }
}

class firewall_node2 {
  firewalld_port { 'Open port 9008 in the public zone':
    ensure   => present,
    zone     => 'public',
    port     => 9008,
    protocol => 'tcp',
  }
}

class firewall_node3 {
  firewalld_port { 'Open port 8097 in the public zone':
    ensure   => present,
    zone     => 'public',
    port     => 8097,
    protocol => 'tcp',
  }
}
```

Create code.pp:  
```
node default {}
node 'stapp01.stratos.xfusioncorp.com' {
  include firewall_node1 
}

node 'stapp02.stratos.xfusioncorp.com' {
  include firewall_node2
}

node 'stapp03.stratos.xfusioncorp.com' {
  include firewall_node3
}
```

SSH to agents and run:  
```
sudo puppet agent -t
```
```
sudo firewall-cmd --reload
```
```
sudo firewall-cmd --list-all --zone=public
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

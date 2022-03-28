The Puppet master and Puppet agent nodes have been set up by the Nautilus DevOps team to perform some testing. In Stratos DC all app servers have been configured as Puppet agent nodes. They want to setup a password less SSH connection between Puppet master and Puppet agent nodes and this task needs to be done using Puppet itself. Below are details about the task:  
```
Create a Puppet programming file games.pp under /etc/puppetlabs/code/environments/production/manifests directory on the Puppet master node i.e on Jump Server. Define a class ssh_node1 for agent node 1 i.e App Server 1, ssh_node2 for agent node 2 i.e App Server 2, ssh_node3 for agent node3 i.e App Server 3. You will need to generate a new ssh key for thor user on Jump Server, that needs to be added on all App Servers.  

Configure a password less SSH connection from puppet master i.e jump host to all App Servers. However, please make sure the key is added to the authorized_keys file of each app's sudo user (i.e tony for App Server 1).  
```  
Notes: :- Before clicking on the Check button please make sure to verify puppet server and puppet agent services are up and running on the respective servers, also please make sure to run puppet agent test to apply/test the changes manually first.  

:- Please note that once lab is loaded, the puppet server service should start automatically on puppet master server, however it can take upto 2-3 minutes to start.  

Solution:  
Create new SSH key for thor:  
```
ssh-keygen
```

Then, create  games.pp under /etc/puppetlabs/code/environments/production/manifests:  
```
$public_key =  'new public key from step above copy it here'

class ssh_node1 {

   ssh_authorized_key { 'tony@stapp01':

     ensure => present,
     user   => 'tony',
     type   => 'ssh-rsa',
     key    => $public_key,
   }
 }

 class ssh_node2 {

   ssh_authorized_key { 'steve@stapp02':
     
     ensure => present,
     user   => 'steve',
     type   => 'ssh-rsa',
     key    => $public_key,
   }
 }

 class ssh_node3 {

   ssh_authorized_key { 'banner@stapp03':

     ensure => present,
     user   => 'banner',
     type   => 'ssh-rsa',
     key    => $public_key,
   }
 }

 node stapp01.stratos.xfusioncorp.com {
   include ssh_node1
 }

 node stapp02.stratos.xfusioncorp.com {
   include ssh_node2
 }

 node stapp03.stratos.xfusioncorp.com {
   include ssh_node3
 }
```

SSH to every app server and run:  
```
puppet agent -t
```

Then, go back to thor@jump_host and try to ssh to every app server. You shouldn't be asked for password.  

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)
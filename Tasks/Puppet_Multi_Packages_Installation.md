Some new changes need to be made on some of the app servers in Stratos Datacenter. There are some packages that need to be installed on the app server 2. We want to install these packages using puppet only.   

a. Puppet master is already installed on Jump Server.   

b. Create a puppet programming file official.pp under /etc/puppetlabs/code/environments/production/manifests on master node i.e on Jump Server and perform below mentioned tasks using the same.    

c. Define a class multi_package_node for agent node 2 i.e app server 2. Install vim-enhanced and zip packages on the agent node 2.   

Notes: :- Please make sure to run the puppet agent test using sudo on agent nodes, otherwise you can face certificate issues. In that case you will have to clean the certificates first and then you will be able to run the puppet agent test.   

:- Before clicking on the Check button please make sure to verify puppet server and puppet agent services are up and running on the respective servers, also please make sure to run puppet agent test to apply/test the changes manually first.   

:- Please note that once lab is loaded, the puppet server service should start automatically on puppet master server, however it can take upto 2-3 minutes to start.    

Solution:  

Create official.pp under /etc/puppetlabs/code/environments/production/manifests:  

```
class multi_package_node {

  package { 'vim-enhanced': ensure => 'installed' }
  package { 'zip': ensure => 'installed' }
}
node 'stapp02.stratos.xfusioncorp.com' {
  include multi_package_node
}

```

ssh to stapp02 and run:  
```
sudo puppet agent -tv   
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)   

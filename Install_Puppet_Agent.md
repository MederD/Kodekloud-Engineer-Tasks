Install Puppet agent on App Server03 and make service is running.  

Solution:  

First of all, enable the Puppet 5 Platform repository:  
```
sudo rpm -Uvh https://yum.puppet.com/puppet5-release-el-7.noarch.rpm  
```

SSH to stapp03 and install puppet:  
```
sudo yum update -y
sudo yum install y puppet-agent
```

Start and enable service:  
```
sudo systemctl start puppet && sudo systemctl enable puppet  
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  


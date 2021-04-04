The Nautilus DevOps team started experimenting with the Puppet server to manage some of their infrastructure in Stratos DC. For testing different scenarios, the team will be using jump host as puppet master. At this point we just need to install puppet server package and ensure its service is up and running. Below you can find more details about the task.  

1.) Install puppetserver package on jump host and start its service.  

2.) Before starting puppetserver service, you might need to change its memory allocation configuration. We recommend to allocating it 512m of memory.  

Note: Please make sure to install puppetserver package only not any other alternate package.  

Solution:
```
rpm -Uvh https://yum.puppet.com/puppet6-release-el-7.noarch.rpm
yum install -y puppetserver
```

Modify /etc/sysconfig/puppetserver:  
```
Replace 2g with the amount of memory you want to allocate to Puppet Server. For example, to allocate 1GB of memory, use JAVA_ARGS="-Xms1g -Xmx1g"; for 512MB, use JAVA_ARGS="-Xms512m -Xmx512m".
```

```
systemctl start puppetserver && systemctl enable puppetserver
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  


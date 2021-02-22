There is some data on App Server 2 in Stratos DC. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes. The DevOps team is working to prepare a Puppet programming file to accomplish this. Below you can find more details about the task.  
Create a Puppet programming file apps.pp under /etc/puppetlabs/code/environments/production/manifests directory on Puppet master node i.e Jump Server and by using puppet file_line resource perform the following tasks.  

We have a file /opt/sysops/apps.txt on App Server 2. Use the Puppet programming file mentioned above to replace line Welcome to Nautilus Industries! to Welcome to xFusionCorp Industries!, no other data should be altered in this file.  

Note: Please perform this task using apps.pp only, do not create any separate inventory file.  

1.) Check the agents on master node:  
```
puppetserver ca list --all  
```

2.) cat > /etc/puppetlabs/code/environments/production/manifests/apps.txt:  
```
file { '/opt/sysops/apps.txt':
  ensure => present,
}->
file_line { 'Replace a line in /opt/sysops/apps.txt':
  path => '/opt/sysops/apps.txt',  
  line => 'Welcome to xFusionCorp Industries!',
  match   => "Welcome to Nautilus Industries!",
}
```

3.) We can confirm changes with cat /opt/sysops/apps.txt  

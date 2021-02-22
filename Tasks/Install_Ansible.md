During their weekly meeting, the Nautilus DevOps team developed automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with Ansible for now due to its simple setup and minimal pre-requisites. The team wanted to start testing with Ansible, so they have decided to use jump host as an Ansible controller to test different kinds of tasks on the rest of the servers.   

Install ansible version 2.9.5 on Jump host.   

Solution:   

Have to upgrade pip, first:
```
pip3 install -U pip
```

Then, install ansible:   
```
sudo pip3 install ansible==2.5.9
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

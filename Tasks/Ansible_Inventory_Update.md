The Nautilus DevOps team has started testing their Ansible playbooks on different servers within the stack. They have placed some playbooks under /home/thor/playbook/ on jump host which they want to test. Some of these playbooks have already been tested on different servers, but now they want to test them on app server 1 in Stratos DC.   However, they first need to create an inventory file so that Ansible can connect to the respective app. Below are some requirements:  

a. Create an ini type Ansible inventory file /home/thor/playbook/inventory on jump host.  

b. Add App Server 1 in this inventory along with required variables that are needed to make it work.  

c. The inventory hostname of host should be the server name as per wiki, for example stapp01 for app server 1 in Stratos DC.  

d. At the end /home/thor/playbook/playbook.yml, playbook should be able to run.  

Note: Validation will try to run playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.   

Solution:  
Have to create inventory file:  
```
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_port=ssh ansible_ssh_pass=Ir0nM@n
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

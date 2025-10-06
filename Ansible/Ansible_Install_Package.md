The Nautilus Application development team wanted to test some applications on app servers in Stratos Datacenter. They shared some pre-requisites with the DevOps team, and packages need to be installed on app servers.  
Since we are already using Ansible for automating such tasks, please perform this task using Ansible as per details mentioned below:  
    Create an inventory file /home/thor/playbook/inventory on jump host and add all app servers in it.  
    Create an Ansible playbook /home/thor/playbook/playbook.yml to install samba package on all  app servers using Ansible yum module.  
    Make sure user thor should be able to run the playbook on jump host.  

Solution:  
`playbook.yml:`   
```ansible
- hosts: all
  become: yes
  tasks:
  - name: install samba
    yum:
      name: samba
      state: present
```
[reference](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/yum_module.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

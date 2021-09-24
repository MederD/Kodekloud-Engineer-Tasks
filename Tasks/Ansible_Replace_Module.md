There is some data on all app servers in Stratos DC. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes they made. The DevOps team is working to prepare an Ansible playbook to accomplish the same. Below you can find more details about the task.

Write a playbook.yml under /home/thor/ansible on jump host, an inventory is already present under /home/thor/ansible directory on Jump host itself. Perform below given tasks using this playbook:  

a. We have a file /opt/data/blog.txt on app server 1. Using Ansible replace module replace string xFusionCorp to Nautilus in that file.  

b. We have a file /opt/data/story.txt on app server 2. Using Ansiblereplace module replace the string Nautilus to KodeKloud in that file.  

c. We have a file /opt/data/media.txt on app server 3. Using Ansible replace module replace string KodeKloud to xFusionCorp Industries in that file.  

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure the playbook works this way without passing any extra arguments.   

Solution:  

```
- hosts: stapp01
  gather_facts: false
  become: yes
  tasks:
  - replace:
      path: /opt/data/blog.txt
      regexp: 'xFusionCorp'
      replace: 'Nautilus'

- hosts: stapp02
  gather_facts: false
  become: yes
  tasks:
  - replace:
      path: /opt/data/story.txt
      regexp: 'Nautilus'
      replace: 'KodeKloud'

- hosts: stapp03
  gather_facts: false
  become: yes
  tasks:
  - replace:
      path: /opt/data/media.txt
      regexp: 'KodeKloud'
      replace: 'xFusionCorp Industries' 
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

The Nautilus DevOps team wants to install and set up a simple httpd web server on all app servers in Stratos DC. Additionally, they want to deploy a sample web page for now using Ansible only. Therefore, prepare the required playbook to complete this task. Find more details about the task below.  

We already have an inventory file under /home/thor/ansible on jump host. Create a playbook.yml under /home/thor/ansible on jump host itself.  

1.) Using the playbook, install httpd web server on all app servers. Additionally, make sure its service should up and running.  

2.) Using blockinfile Ansible module add some content in /var/www/html/index.html file. Below is the content:  
```
    Welcome to XfusionCorp!  

    This is Nautilus sample file, created using Ansible!  

    Please do not modify this file manually!  
```
3.) The /var/www/html/index.html file's user and group owner should be apache on all app servers.  

4.) The /var/www/html/index.html file's permissions should be 0744 on all app servers.    

Note:   

i. Validation will try to run playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.   

ii. Do not use any custom or empty marker for blockinfile module.   


Solution:  
```
- hosts: all
  gather_facts: false
  become: yes
  tasks:
  - name: httpd install
    yum:
      name: httpd
      state: present
    
  - name: Start service
    service:
      name: httpd
      enabled: yes
      state: started
      
  - name: Create directory
    file:
      path: /var/www/html
      state: directory
      owner: apache
      group: apache

  - name: Create index.html
    file: 
      path: /var/www/html/index.html
      state: touch
      owner: apache
      group: apache
      mode: '0744'

  - name: blockinfile part
    blockinfile:
      path: /var/www/html/index.html
      block: |
       Welcome to XfusionCorp!

       This is Nautilus sample file, created using Ansible!

       Please do not modify this file manually!
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

The Nautilus DevOps team want to install and set up a simple httpd web server on all app servers in Stratos DC. They also want to deploy a sample web page using Ansible. Therefore, write the required playbook to complete this task as per details mentioned below.  

We already have an inventory file under /home/thor/ansible directory on jump host. Write a playbook playbook.yml under /home/thor/ansible directory on jump host itself. Using the playbook perform below given tasks:  

    Install httpd web server on all app servers, and make sure its service is up and running.  

    Create a file /var/www/html/index.html with content:  

This is a Nautilus sample file, created using Ansible!  

    Using lineinfile Ansible module add some more content in /var/www/html/index.html file. Below is the content:  

Welcome to Nautilus Group!  

Also make sure this new line is added at the top of the file.  

    The /var/www/html/index.html file's user and group owner should be apache on all app servers.  

    The /var/www/html/index.html file's permissions should be 0744 on all app servers.  

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure the playbook works this way without passing any extra arguments.  

Solution:  
```
- hosts: all
  gather_facts: false
  become: yes
  become_user: root
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
    copy:
      content: |
        This is a Nautilus sample file, created using Ansible! 
      dest: /var/www/html/index.html
 
  - name: lineinfile part
    lineinfile:
      path: /var/www/html/index.html
      line: 'Welcome to Nautilus Group!'
      instertbefore: 'BOF'
      state: present

  - name: file permissions
    file:
      path: /var/www/html/index.html
      owner: apache
      group: apache
      mode: '0744' 

```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  
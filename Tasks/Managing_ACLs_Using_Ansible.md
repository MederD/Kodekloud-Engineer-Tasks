There are some files that need to be created on all app servers in Stratos DC. The Nautilus DevOps team want these files to be owned by user root only; however, they also want that app-specific user to have a set of permissions to these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.  

Create a playbook.yml under /home/thor/ansible on jump host, an inventory file is already present under /home/thor/ansible on Jump Server itself.  

Create an empty file blog.txt under /opt/itadmin/ directory on app server 1. Set some acl properties for this file. Using acl provide read '(r)' permissions to group tony (i.e entity is tony and etype is group).  

Create an empty file story.txt under /opt/itadmin/ directory on app server 2. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to user steve (i.e entity is steve and etype is user).  

Create an empty file media.txt under /opt/itadmin/ on app server 3. Set some acl properties for this file. Using acl provide read + write '(rw)' permissions to group banner (i.e entity is banner and etype is group).  

Note: Validation will try to run playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.  

Solution:  
```
---
- hosts: stapp01
  become: yes
  tasks:
    - name: Touch empty file stapp01
      file:
        path: "/opt/itadmin/blog.txt"
        state: touch 
        owner: root
    - name: Configure ACL
      acl: 
        path: "/opt/itadmin/blog.txt"
        entity: tony
        etype: group
        permissions: r
        state: present
        
- hosts: stapp02
  become: yes
  tasks:
    - name: Touch empty file stapp02
      file:
        path: "/opt/itadmin/story.txt"
        state: touch 
        owner: root
    - name: Configure ACL
      acl: 
        path: "/opt/itadmin/story.txt"
        entity: steve
        etype: user
        permissions: rw
        state: present
        
- hosts: stapp03
  become: yes
  tasks:
    - name: Touch empty file stapp03
      file:
        path: "/opt/itadmin/media.txt"
        state: touch 
        owner: root
    - name: Configure ACL
      acl: 
        path: "/opt/itadmin/media.txt"
        entity: banner
        etype: group
        permissions: rw
        state: present
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

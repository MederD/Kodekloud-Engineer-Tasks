The Nautilus DevOps team is working to test several Ansible modules on servers in Stratos DC. Recently they wanted to test the file creation on remote hosts using Ansible. Find below more details about the task:  

a. Create an inventory file ~/playbook/inventory on jump host and add all app servers in it.  

b. Create a playbook ~/playbook/playbook.yml to create a blank file /opt/webapp.txt on all app servers.  

c. The /opt/webapp.txt file permission must be 0644.  

d. The user/group owner of file /opt/webapp.txt must be tony on app server 1, steve on app server 2 and banner on app server 3.  

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml, so please make sure the playbook works this way without passing any extra arguments.  

Solution:  
Create inventory file:  
```
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n 
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n
```

Create playbook.yml:  
```
- hosts: all
  become: yes
  tasks:
  - name: Create empty file
    file:
      path: /opt/webapp.txt
      state: touch
      owner: "{{ ansible_user }}"
      group: "{{ ansible_user }}"
      mode: "0644"
```

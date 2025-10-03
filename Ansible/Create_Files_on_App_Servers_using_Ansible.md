The Nautilus DevOps team is testing various Ansible modules on servers in Stratos DC. They're currently focusing on file creation on remote hosts using Ansible. Here are the details:  
a. Create an inventory file ~/playbook/inventory on jump host and include all app servers.  
b. Create a playbook ~/playbook/playbook.yml to create a blank file /opt/nfsdata.txt on all app servers.  
c. Set the permissions of the /opt/nfsdata.txt file to 0777.  
d. Ensure the user/group owner of the /opt/nfsdata.txt file is tony on app server 1, steve on app server 2 and banner on app server 3.  


Solution:  
`inventory:`  
```ansible
[app_servers]
stapp01 ansible_user=tony ansible_password=Ir0nM@n ansible_connection=ssh
stapp02 ansible_user=steve ansible_password=Am3ric@ ansible_connection=ssh
stapp03 ansible_user=banner ansible_password=BigGr33n ansible_connection=ssh
```

`playbook.yml:`  
```ansible
- hosts: all
  become: yes
  tasks:
  - name: Create empty file
    file:
      path: /opt/nfsdata.txt
      state: touch
      owner: "{{ ansible_user }}"
      group: "{{ ansible_user }}"
      mode: "0777"
```
`check:`  
```bash
ssh tony@stapp01 "/bin/bash -c 'ls -l /opt/'"
```
[reference](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

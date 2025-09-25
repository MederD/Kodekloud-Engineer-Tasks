The Nautilus DevOps team needs to copy data from the jump host to all application servers in Stratos DC using Ansible. Execute the task with the following details:  
a. Create an inventory file /home/thor/ansible/inventory on jump_host and add all application servers as managed nodes.  
b. Create a playbook /home/thor/ansible/playbook.yml on the jump host to copy the /usr/src/devops/index.html file to all application servers, placing it at /opt/devops.  


Solution:  
`inventory`  
```ansible
[app_servers]
stapp01 ansible_user=tony ansible_password=Ir0nM@n ansible_connection=ssh
stapp02 ansible_user=steve ansible_password=Am3ric@ ansible_connection=ssh
stapp03 ansible_user=banner ansible_password=BigGr33n ansible_connection=ssh
```
`playbook.yml`  
```yaml
- name: Copy files
  hosts: app_servers
  become: yes
  tasks:
    - name: Copy file
      copy:
        src: /usr/src/devops/index.html
        dest: /opt/devops/
```
[reference](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html#examples)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

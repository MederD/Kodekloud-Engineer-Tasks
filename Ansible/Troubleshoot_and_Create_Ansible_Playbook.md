An Ansible playbook needs completion on the jump host, where a team member left off. Below are the details:  
    The inventory file /home/thor/ansible/inventory requires adjustments. The playbook must run on App Server 2 in Stratos DC. Update the inventory accordingly.  
    Create a playbook /home/thor/ansible/playbook.yml. Include a task to create an empty file /tmp/file.txt on App Server 2.  

Solution:  
`inventory:`  
```text
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_password=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
`playbook.yml:`  
```ansible
---
- name: Empty File
  hosts: stapp02
  become: false
  tasks:
    - name: ensure file exists
      copy:
        content: ""
        dest: /tmp/file.txt
        force: false
```
```bash
ssh-keygen
ssh-copy-id steve@stapp02
```
[reference](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

The Nautilus DevOps team has some data on each app server in Stratos DC that they want to copy to a different location. However, they want to create an archive of the data first,  
then they want to copy the same to a different location on the respective app server. Additionally, there are some specific requirements for each server.  
Perform the task using Ansible playbook as per requirements mentioned below:  
Create a playbook named playbook.yml under /home/thor/ansible directory on jump host, an inventory file is already placed under /home/thor/ansible/ directory on Jump Server itself.  
Create an archive ecommerce.tar.gz (make sure archive format is tar.gz) of /usr/src/itadmin/ directory ( present on each app server ) and copy it to /opt/itadmin/ directory on all app servers.   
The user and group owner of archive ecommerce.tar.gz should be tony for App Server 1, steve for App Server 2 and banner for App Server 3.  


Solution:  
`playbook.yml:`  
```ansible
- hosts: all
  become: yes
  tasks:
  - name: Compress directory /usr/src/itadmin to /tmp/ecommerce.tar.gz
    archive:
      path: /usr/src/itadmin/
      dest: /tmp/ecommerce.tar.gz
      format: gz
    register: archive_result

  - name: Copy archive to /opt/itadmin/
    copy:
      src: /tmp/ecommerce.tar.gz
      dest: /opt/itadmin/ecommerce.tar.gz
      remote_src: yes

  - name: Set owner and group of /opt/itadmin/ecommerce.tar.gz
    file:
      path: /opt/itadmin/ecommerce.tar.gz
      owner: "{{ ansible_user }}"
      group: "{{ ansible_user }}"
```
[reference](https://docs.ansible.com/ansible/latest/collections/community/general/archive_module.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

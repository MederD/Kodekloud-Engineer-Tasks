One of the DevOps team members has created an ZIP archive on jump host in Stratos DC that needs to be extracted and copied over to all app servers in Stratos DC itself. Because this is a routine task, the Nautilus DevOps team has suggested automating it. We can use Ansible since we have been using it for other automation tasks. Below you can find more details about the task:

We have an inventory file under /home/thor/ansible directory on jump host, which should have all the app servers added already.  

There is a ZIP archive /usr/src/dba/xfusion.zip on jump host.  

Create a playbook.yml under /home/thor/ansible/ directory on jump host itself to perform the below given tasks.  

    Unzip /usr/src/dba/xfusion.zip archive in /opt/dba/ location on all app servers.  

    Make sure the extracted data must has the respective sudo user as their user and group owner, i.e tony for app server 1, steve for app server 2, banner for app server 3.  

    The extracted data permissions must be 0755  

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way, without passing any extra arguments.  

Solution:  

```

- name: playbook for file manipulation
  hosts: stapp01
  become: yes
  
  tasks:
  - name: Unarchive a file that needs to be downloaded (added in 2.0)
    unarchive:
      src: /usr/src/dba/xfusion.zip
      dest: /opt/dba
      mode: 0755
      owner: tony
      group: tony

- name: playbook for file manipulation 2
  hosts: stapp02
  become: yes
  
  tasks:
  - name: Unarchive a file that needs to be downloaded (added in 2.0)
    unarchive:
      src: /usr/src/dba/xfusion.zip
      dest: /opt/dba
      mode: 0755
      owner: steve
      group: steve

- name: playbook for file manipulation 3
  hosts: stapp03
  become: yes
  
  tasks:
  - name: Unarchive a file that needs to be downloaded (added in 2.0)
    unarchive:
      src: /usr/src/dba/xfusion.zip
      dest: /opt/dba
      mode: 0755
      owner: banner
      group: banner

```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  



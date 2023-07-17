The Nautilus DevOps team had a discussion about, how they can train different team members to use Ansible for different automation tasks. There are numerous ways to perform a particular task using Ansible, but we want to utilize each aspect that Ansible offers. The team wants to utilise Ansible's conditionals to perform the following task:

An inventory file is already placed under /home/thor/ansible directory on jump host, with all the Stratos DC app servers included.

Create a playbook /home/thor/ansible/playbook.yml and make sure to use Ansible's when conditionals statements to perform the below given tasks.

    Copy blog.txt file present under /usr/src/security directory on jump host to App Server 1 under /opt/security directory. Its user and group owner must be user tony and its permissions must be 0777 .

    Copy story.txt file present under /usr/src/security directory on jump host to App Server 2 under /opt/security directory. Its user and group owner must be user steve and its permissions must be 0777 .

    Copy media.txt file present under /usr/src/security directory on jump host to App Server 3 under /opt/security directory. Its user and group owner must be user banner and its permissions must be 0777 .

NOTE: You can use ansible_nodename variable from gathered facts with when condition. Additionally, please make sure you are running the play for all hosts i.e use - hosts: all.

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml, so please make sure the playbook works this way without passing any extra arguments.

**Solution**:  
Create playbook.yaml:
```
---
- hosts: all
  become: yes
  gather_facts: yes
  tasks:
    - name: Copy blog.txt file with owner and permissions
      copy:
        src: /usr/src/security/blog.txt
        dest: /opt/security/blog.txt
        owner: tony
        group: tony
        mode: "0777"
      when: inventory_hostname == "stapp01"

    - name: Copy story.txt file with owner and permissions
      copy:
        src: /usr/src/security/story.txt
        dest: /opt/security/story.txt
        owner: steve
        group: steve
        mode: "0777"
      when: inventory_hostname == "stapp02"

    - name: Copy media.txt file with owner and permissions
      copy:
        src: /usr/src/security/media.txt
        dest: /opt/security/media.txt
        owner: banner
        group: banner
        mode: "0777"
      when: inventory_hostname == "stapp03"
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

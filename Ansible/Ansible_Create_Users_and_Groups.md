Several new developers and DevOps engineers just joined the xFusionCorp industries. They have been assigned the Nautilus project, and as per the onboarding process we need to create user accounts for new joinees on at least one of the app servers in Stratos DC.  
We also need to create groups and make new users members of those groups. We need to accomplish this task using Ansible. Below you can find more information about the task.  
There is already an inventory file ~/playbooks/inventory on jump host.  
On jump host itself there is a list of users in ~/playbooks/data/users.yml file and there are two groups — admins and developers —that have list of different users.  
Create a playbook ~/playbooks/add_users.yml on jump host to perform the following tasks on app server 2 in Stratos DC.  
a. Add all users given in the users.yml file on app server 2.  
b. Also add developers and admins groups on the same server.  
c. As per the list given in the users.yml file, make each user member of the respective group they are listed under.  
d. Make sure home directory for all of the users under developers group is /var/www (not the default i.e /var/www/{USER}). Users under admins group should use the default home directory (i.e /home/devid for user devid).  
e. Set password YchZHRcLkL for all of the users under developers group and Rc5C9EyvbU for of the users under admins group. Make sure to use the password given in the ~/playbooks/secrets/vault.txt file as Ansible vault password to encrypt the original password strings.  
You can use ~/playbooks/secrets/vault.txt file as a vault secret file while running the playbook (make necessary changes in ~/playbooks/ansible.cfg file).  
f. All users under admins group must be added as sudo users. To do so, simply make them member of the wheel group as well.  
Note: Validation will try to run the playbook using command ansible-playbook -i inventory add_users.yml so please make sure playbook works this way, without passing any extra arguments.  

Solution:  
`add_users.yml:`  
```ansible
---
- name: Add users and groups
  hosts: stapp02
  become: yes

  vars_files:
    - data/users.yml

  vars:
    dev_password: "{{ 'YchZHRcLkL' | password_hash('sha512') }}"
    admin_password: "{{ 'Rc5C9EyvbU' | password_hash('sha512') }}"

  tasks:
    - name: Ensure groups exist
      ansible.builtin.group:
        name: "{{ item }}"
        state: present
      loop:
        - admins
        - developers
        - wheel

    - name: Create admin users
      ansible.builtin.user:
        name: "{{ item }}"
        group: admins
        groups: wheel
        password: "{{ admin_password }}"
        shell: /bin/bash
        create_home: yes
      loop: "{{ admins }}"

    - name: Create developer users
      ansible.builtin.user:
        name: "{{ item }}"
        group: developers
        password: "{{ dev_password }}"
        shell: /bin/bash
        create_home: yes
        home: /var/www
      loop: "{{ developers }}"
```

[reference](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_loops.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

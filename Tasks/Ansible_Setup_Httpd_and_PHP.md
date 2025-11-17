Nautilus Application development team wants to test the Apache and PHP setup on one of the app servers in Stratos Datacenter. They want the DevOps team to prepare an Ansible playbook to accomplish this task. Below you can find more details about the task.  
There is an inventory file ~/playbooks/inventory on jump host.  
Create a playbook ~/playbooks/httpd.yml on jump host and perform the following tasks on App Server 3.  
a. Install httpd and php packages (whatever default version is available in yum repo).  
b. Change default document root of Apache to /var/www/html/myroot in default Apache config /etc/httpd/conf/httpd.conf. Make sure /var/www/html/myroot path exists (if not please create the same).  
c. There is a template ~/playbooks/templates/phpinfo.php.j2 on jump host. Copy this template to the Apache document root you created as phpinfo.php file and make sure user owner and the group owner for this file is apache user.  
d. Start and enable httpd service.  
Note: Validation will try to run the playbook using command ansible-playbook -i inventory httpd.yml, so please make sure the playbook works this way without passing any extra arguments.  

Solution:  
`httpd.yml:`  
```ansible
- hosts: stapp03
  become: yes
  tasks:
    - name: Install packages
      yum:
        name:
          - httpd
          - php
        state: present

    - name: Ensure document exists
      file:
        path: /var/www/html/myroot
        state: directory
        owner: apache
        group: apache
        mode: '0755'

    - name: Change config in httpd.conf
      replace:
        path: /etc/httpd/conf/httpd.conf
        regexp: '^DocumentRoot\s+"[^"]+"'
        replace: 'DocumentRoot "/var/www/html/myroot"'
      notify: restart httpd

    - name: Copy phpinfo.php
      template:
        src: /home/thor/playbooks/templates/phpinfo.php.j2
        dest: /var/www/html/myroot/phpinfo.php
        owner: apache
        group: apache
        mode: '0644'

    - name: Start and enable httpd service
      service:
        name: httpd
        state: started
        enabled: true

  handlers:
    - name: restart httpd
      service:
        name: httpd
        state: restarted
```
[reference](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

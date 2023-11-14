The Nautilus system admins team recently deployed a web UI application for their backup utility running on the Nautilus backup server in Stratos Datacenter. The application is running on port 5000. They have firewalld installed on that server. The requirements that have come up include the following:
Open all incoming connection on 5000/tcp port. Zone should be public.

Solution:  

- SSH to the server
- Run:
  ```
  sudo firewall-cmd --zone=public --add-port=5000/tcp
  ```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

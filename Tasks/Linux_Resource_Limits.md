On our Storage server in Stratos Datacenter we are having some issues where nfsuser user is holding hundred of processes, which is degrading the performance of the server. Therefore, we have a requirement to limit its maximum processes. Please set its maximum process limits as below:
a. soft limit = 1027
b. hard_limit = 2024

Solution:  
- SSH to server
- Edit the /etc/security/limits.conf, add:
  ```
  ----
  nfsuser soft nproc 1024
  nfsuser hard nproc 2024
  ```


[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

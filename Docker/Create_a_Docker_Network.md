The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:  
a. Create a docker network named as beta on App Server 3 in Stratos DC.  
b. Configure it to use macvlan drivers.  
c. Set it to use subnet 192.168.30.0/24 and iprange 192.168.30.0/24.  

Solution:  
1. SSH to the app server requested - it may be a different app server for you.

    ```bash
    ssh banner@stapp03
    ```

2. Become root to save typing `sudo` on every command

    ```bash
    sudo su
    ```

3. Creata network

    ```bash
    docker network create -d macvlan --subnet 192.168.30.0/24  --ip-range 192.168.30.0/24 beta
    ```

4.  Inspect the network

    ```bash
    docker network inspect beta
    ```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 3 in Stratos Datacenter.  
Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:  
a. Install apache2 in kkloud container using apt that is running on App Server 3 in Stratos Datacenter.  
b. Configure Apache to listen on port 6200 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.  
c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.  

1. SSH to the app server requested - it may be a different app server for you.

    ```bash
    ssh banner@stapp03
    ```

2. Become root to save typing `sudo` on every command

    ```bash
    sudo su
    ```

3. Update and install `apache2`

    ```bash
    apt update
    apt install apache2 -y
    ```

4.  Configure apache

    1. Edit `/etc/apache2/ports.conf` with vi.
    2. Change the `Listen` line to `6200` instead of `80`.
    3. Edit `/etc/apache2/sites-enabled/000-default.conf` with vi.
    4. Change the `<VirtualHost` line  to `<VirtualHost *:6200>`
    5. Save and exit.
       
5. Start the service and check status
   ```bash
   service apache2 restart
   service apache2 status
   ``` 

6. Check if apache is working
   ```bash
   curl http://localhost:6200
   ```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

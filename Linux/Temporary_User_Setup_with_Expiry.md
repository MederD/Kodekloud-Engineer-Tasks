As part of the temporary assignment to the Nautilus project, a developer named mariyam requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:  
Create a user named mariyam on App Server 3 in Stratos Datacenter. Set the expiry date to 2024-02-17, ensuring the user is created in lowercase as per standard protocol.  

Solution:  
```
ssh banner@stapp03
sudo useradd mariyam -e 2024-02-17
```
check:  
```
[banner@stapp03 ~]$ sudo chage -l mariyam
Last password change                                    : Jul 14, 2025
...
Account expires                                         : Feb 17, 2024
```
[reference](https://www.baeldung.com/linux/temporary-user-account)    
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main) 

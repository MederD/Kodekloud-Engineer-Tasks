New tools have been installed on the app servers in Stratos Datacenter. Some of these tools can only be managed from the graphical user interface. Therefore, there are some requirements for these app servers as below.
On all App servers in Stratos Datacenter, change the default runlevel so that they can boot in GUI (graphical user interface) by default. Please do not try to reboot these servers after completing this task.

Solution:  
SSH to app servers and run:
```
systemctl get-default
```

which will return the run level, it's most probably set to "multi-user.target".

next, run:   
```
sudo systemctl set-default graphical.target
```

it will change the default run level to GUI

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

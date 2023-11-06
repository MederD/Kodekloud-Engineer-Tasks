To stick with the security compliances, the Nautilus project team has decided to apply some restrictions on crontab access so that only allowed users can create/update the cron jobs. Limit crontab access to below specified users on App Server 3.
Allow crontab access to ravi user and deny the same to eric user.

Solution:  
- SSH to "stapp03"
- create file /etc/cron.allow and add "ravi"
- create file /etc/cron.deny and add "ammar"
- sudo systemctl restart crond.service

  [back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

There is a requirement to create a Jenkins job to automate the database backup. Below you can find more details to accomplish this task:  
Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.  
    Create a Jenkins job named database-backup.  
    Configure it to take a database dump of the kodekloud_db01 database present on the Database server in Stratos Datacenter, the database user is kodekloud_roy and password is asdfgdsd.  
    The dump should be named in db_$(date +%F).sql format, where date +%F is the current date.  
    Copy the db_$(date +%F).sql dump to the Backup Server under location /home/clint/db_backups.  
    Further, schedule this job to run periodically at */10 * * * * (please use this exact schedule format).  

Solution:  
- Install SSH plugins  
- Create credentials for "peter" and "clint"
- Create ssh host connections
- Create job:
  - add schedule
  - Add "Execute shell script on remote host using SSH" under Build, add below script:
    ```bash
    mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > db_$(date +%F).sql
    echo Sp!dy | sudo -S yum install sshpass -y
    echo Sp!dy | sudo -S sshpass -p H@wk3y3 scp -o StrictHostKeyChecking=no -r /home/peter/*sql clint@stbkp01:/home/clint/db_backups
    ```
[reference](https://www.jenkins.io/doc/book/using/using-credentials/)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

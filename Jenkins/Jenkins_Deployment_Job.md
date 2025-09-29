The Nautilus development team had a meeting with the DevOps team where they discussed automating the deployment of one of their apps using Jenkins (the one in Stratos Datacenter).  
They want to auto deploy the new changes in case any developer pushes to the repository. As per the requirements mentioned below configure the required Jenkins job.  
Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and Adm!n321 password.  
Similarly, you can access the Gitea UI using Gitea button, username and password for Git is sarah and Sarah_pass123 respectively.  
Under user sarah you will find a repository named web that is already cloned on the Storage server under sarah's home. sarah is a developer who is working on this repository.  
1. Install httpd (whatever version is available in the yum repo by default) and configure it to serve on port 8080 on All app servers. You can make it part of your Jenkins job or you can do this step manually on all app servers.  
2. Create a Jenkins job named nautilus-app-deployment and configure it in a way so that if anyone pushes any new change to the origin repository in master branch,
3. the job should auto build and deploy the latest code on the Storage server under /var/www/html directory. Since /var/www/html on Storage server is shared among all apps.  
 Before deployment, ensure that the ownership of the /var/www/html directory is set to user sarah, so that Jenkins can successfully deploy files to that directory.  
4. SSH into Storage Server using sarah user credentials mentioned above. Under sarah user's home you will find a cloned Git repository named web.
5. Under this repository there is an index.html file, update its content to Welcome to the xFusionCorp Industries, then push the changes to the origin into master branch.
6. This push must trigger your Jenkins job and the latest changes must be deployed on the servers, also make sure it deploys the entire repository content not only index.html file.

Solution:  
1 - Install SSH, pipeline and Git plugins  
2 - Create credentials for all servers   
3 - Create job `httpds` to install httpd on remote host:  
```bash  
    echo <PASSWORD> | sudo -S yum install httpd -y  
    echo <PASSWORD> | sudo -S sed -i 's/^Listen 80$/Listen 8080/' /etc/httpd/conf/httpd.conf  
    echo <PASSWORD> | sudo -S systemctl restart httpd  
    echo <PASSWORD> | sudo -S systemctl enable httpd  
 ```  
4 - Create job `nautilus-app-deployment`:    
  -  General > Source Code Management > Git    
  -  Build Triggers > Trigger builds remotely
    
5 - Create webhook on Gitea.  
6 - SSH to `ststor01` as Sarah and do the required changes.  

[reference](https://httpd.apache.org/docs/current/configuring.html)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

The DevOps team at xFusionCorp Industries is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:  
1. Install jenkins on jenkins server using yum utility only, and start its service. You might face timeout issue while starting the Jenkins service, please refer this link for help.  
2. Jenkin's admin user name should be theadmin, password should be Adm!n321, full name should be Anita and email should be anita@jenkins.stratos.xfusioncorp.com.  
Note:  
1. For this task, access the Jenkins server by SSH using the root user and password S3curePass from the jump host.  
2. After Jenkins server installation, click the Jenkins button on the top bar to access the Jenkins UI and follow on-screen instructions to create an admin user.

Solution:  
```
ssh root@jenkins
yum install wget
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
# Add required dependencies for the jenkins package
sudo yum install fontconfig java-21-openjdk
sudo yum install jenkins
sudo systemctl daemon-reload
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```
[reference](https://www.jenkins.io/doc/book/installing/linux/#red-hat-centos)  
[back](https://github.com/MederD/Kodekloud-Engineer-Task)

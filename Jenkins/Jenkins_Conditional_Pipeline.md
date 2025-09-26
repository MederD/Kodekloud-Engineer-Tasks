The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline.  
They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:  
Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username admin and password Adm!n321.  
Similarly, click on the Gitea button on the top bar to access the Gitea UI. Login using username sarah and password Sarah_pass123.  
There under user sarah you will find a repository named web_app that is already cloned on Storage server under /var/www/html. sarah is a developer who is working on this repository.  
Add a slave node named Storage Server. It should be labeled as ststor01 and its remote root directory should be /var/www/html.  
We have already cloned repository on Storage Server under /var/www/html.  
Apache is already installed on all app Servers its running on port 8080.  
Create a Jenkins pipeline job named datacenter-webapp-job (it must not be a Multibranch pipeline) and configure it to:  
Add a string parameter named BRANCH.  
It should conditionally deploy the code from web_app repository under /var/www/html on Storage Server, as this location is already mounted to the document root /var/www/html of app servers.  
The pipeline should have a single stage named Deploy ( which is case sensitive ) to accomplish the deployment.  
The pipeline should be conditional, if the value master is passed to the BRANCH parameter then it must deploy the master branch, on the other hand if the value feature is passed to the BRANCH parameter then it must deploy the feature branch.  
LB server is already configured. You should be able to see the latest changes you made by clicking on the App button.  
Please make sure the required content is loading on the main URL https://<LBR-URL> i.e there should not be a sub-directory like https://<LBR-URL>/web_app etc.  


Solution:  
1 - Install SSH, pipeline and Git plugins  
2 - Create credentials for `ststor01` and Git  
3 - SSH to `ststor01` and change ownership of root directory  
4 - Create host  
5 - Create job to install java on remote host, task #3 can be done in this step, too.  
6 - Create node  
7 - Create pipeline:  
```
pipeline {
    agent { label 'ststor01' }
    parameters {
        string(name: 'BRANCH', defaultValue: 'master')
    }
    stages {
        stage('Deploy') {
            steps {
                script {
                    def branch = params.BRANCH
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: "*/${branch}"]],
                        userRemoteConfigs: [[
                            url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git',
                            credentialsId: 'git-credentials'
                        ]],
                        extensions: []
                    ])
                    
                    sh 'mkdir -p /var/www/html/workspace'
                    sh 'cp -pr /var/www/html/workspace/datacenter-webapp-job/* /var/www/html/'

                    if (branch == 'feature') {
                        sh 'mkdir -p /var/www/html/workspace'
                        sh 'cp -pr /var/www/html/workspace/datacenter-webapp-job/* /var/www/html/'
                    }
                }
            }
        }
    }
}

```
[reference](https://www.jenkins.io/blog/2017/01/19/converting-conditional-to-pipeline/)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)

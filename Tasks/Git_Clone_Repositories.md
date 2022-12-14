DevOps team created a new Git repository last week; however, as of now no team is using it. The Nautilus application development team recently asked for a copy of that repo on Storage server in Stratos DC. Please clone the repo as per details shared below:

    The repo that needs to be cloned is /opt/official.git

    Clone this git repository under /usr/src/kodekloudrepos directory. Please do not try to make any changes in repo.
    
Solution:  
1. SSH to stsor01  
2. cd to /usr/src/kodekloudrepos  
3. Run command:  
```
git clone /opt/official.git
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

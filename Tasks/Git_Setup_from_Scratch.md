Some new developers have joined xFusionCorp Industries and have been assigned Nautilus project. They are going to start development on a new application, and some pre-requisites have been shared with the DevOps team to proceed with. Please note that all tasks need to be performed on storage server in Stratos DC.  

a. Install git, set up any values for user.email and user.name globally and create a bare repository /opt/demo.git.  

b. There is an update hook (to block direct pushes to master branch) under /tmp on storage server itself; use the same to block direct pushes to master branch in /opt/demo.git repo.  

c. Clone /opt/demo.git repo in /usr/src/kodekloudrepos/demo directory.  

d. Create a new branch xfusioncorp_demo in repo that you cloned in /usr/src/kodekloudrepos.  

e. There is a readme.md file in /tmp on storage server itself; copy that to repo, add/commit in the new branch you created, and finally push your branch to origin.  

f. Also create master branch from your branch and remember you should not be able to push to master as per hook you have set up.  

Solution:  
a. Install git, set up any values for user.email and user.name globally and create a bare repository /opt/demo.git.  
```
1.) ssh natasha@ststor01  
2.) sudo su  
3.) yum install git -y
4.) git config --global --add user.name natasha
    git config --global --add user.email natasha@xfusioncorp.com
5.) mkdir /opt/demo.git && cd $_
6.) git init -- bare  
```

b. Copy update hook:  
```
cp /tmp/update /opt/demo.git/hooks/update  
cp /tmp/update /usr/src/kodekloudrepos/demo/.git/hooks/update  
```

c. git clone /opt/demo.git /usr/src/kodekloudrepos/demo   

d. cd /usr/src/kodekloud/demo:  
```
git checkout -b xfusoincorp_demo
```

e. Copy /tmp/readme.md /usr/src/kodekloudrepos/demo/readme.md  
```
git add readme.md  
git commit -m "commit"  
git push origin xfusioncorp_demo  
```

f. Create master branch:  
```
git checkout -b master  
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  




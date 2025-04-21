The Nautilus application development team has been working on a project repository /opt/beta.git. This repo is cloned at `/usr/src/kodekloudrepos` on storage server in Stratos DC. They recently shared the following requirements with DevOps team:  
One of the developers is working on feature branch and their work is still in progress, however there are some changes which have been pushed into the master branch, the developer now wants to rebase the `feature` branch with the `master` branch without loosing any data from the feature branch, also they don't want to add any merge commit by simply merging the master branch into the feature branch. Accomplish this task as per requirements mentioned.  
Also remember to push your changes once done.  

Solution:  
```
ssh natasha@ststor01
sudo su
cd /usr/src/kodekloudrepos/beta
git log --oneline
git log master --oneline
git rebase origin/master
git log --oneline
git push origin feature --force
```

[reference](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

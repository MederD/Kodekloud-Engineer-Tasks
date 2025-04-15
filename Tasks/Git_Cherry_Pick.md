The Nautilus application development team has been working on a project repository /opt/news.git. This repo is cloned at `/usr/src/kodekloudrepos` on storage server in Stratos DC. They recently shared the following requirements with the DevOps team:
There are two branches in this repository, `master` and `feature`. One of the developers is working on the feature branch and their work is still in progress,  
however they want to merge one of the commits from the feature branch to the master branch, the message for the commit that needs to be merged into master is `Update info.txt`. Accomplish this task for them, also remember to push your changes eventually.  

Solution:  
```
ssh natasha@ststor01
sudo su
git log
git checkout master
git cherry-pick 15c8ac623342356508f1c9e5187268975f9617e0 -> It might be different in your case, find commit with text 'Update info.txt'
git push origin master
git log
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

The Nautilus application development team has been working on a project repository /opt/cluster.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:

a. Create a new branch devops in /usr/src/kodekloudrepos/cluster repo from master and copy the /tmp/index.html file (on storage server itself). Add/commit this file in the new branch and merge back that branch to the master branch. Finally, push the changes to origin for both of the branches.

Solution:  
```
ssh to natasha@ststor01
cd /usr/src/kodekloudrepos/cluster 
git checkout -b devops
cp /tmp/index.html ./
git add .
git commit -m "adding index.html"
git pushorigin devops
git checkout master
git merge devops
git push origin master
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

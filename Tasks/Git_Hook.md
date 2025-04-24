The Nautilus application development team was working on a git repository `/opt/apps.git` which is cloned under `/usr/src/kodekloudrepos` directory present on Storage server in Stratos DC.  
The team want to setup a hook on this repository, please find below more details:  
Merge the `feature` branch into the `master` branch, but before pushing your changes complete below point.  
Create a `post-update` hook in this git repository so that whenever any changes are pushed to the `master` branch, it creates a release tag with name `release-2023-06-15`, where 2023-06-15 is supposed to be the `current` date.  
`For example if today is 20th June, 2023 then the release tag must be release-2023-06-20`. Make sure you test the hook at least once and create a release tag for today's release.  
Finally remember to push your changes.  

Solution:  
```
ssh natasha@ststor01
sudo su
vi /opt/apps.git/hooks/post-update
```
Script:  
```
#!/bin/bash
cd /opt/apps.git
git_tag=release-$(date "+%Y-%m-%d")
git tag $git_tag
git push origin
```
```
chmod +x /opt/apps.git/hooks/post-update
cd/ usr/src/kodekloudrepos/apps
git switch master
git merge feature
git push
git fetch --tags
git tag
```

[reference](https://www.atlassian.com/git/tutorials/git-hooks)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)


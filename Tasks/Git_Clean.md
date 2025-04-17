The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/media present on Storage server in Stratos DC.  
One of the developers mistakenly created a couple of files under this repository, but now they want to clean this repository without adding/pushing any new files. Find below more details:  
Clean the `/usr/src/kodekloudrepos/media` git repository without adding/pushing any new files, make sure git status is clean.  

Solution:  
```
ssh natasha@ststor01
sudo su
cd /usr/src/kodekloudrepos/media
git status
git clean -fd
git status
```
[reference](https://www.atlassian.com/git/tutorials/undoing-changes/git-clean)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

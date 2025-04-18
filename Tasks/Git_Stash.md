The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/demo present on Storage server in Stratos DC.  
One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:  
Look for the stashed changes under `/usr/src/kodekloudrepos/demo` git repository, and restore the stash with `stash@{1}` identifier. Further, commit and push your changes to the origin.  

Solution:  
```
ssh natasha@ststor01
sudo su
git stash list
git stash pop stash@{1}
git status
git commit -m "adding stached change"
git log --oneline
git push
```

[reference](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

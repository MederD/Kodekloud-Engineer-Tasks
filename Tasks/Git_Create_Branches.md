Nautilus developers are actively working on one of the project repositories, `/usr/src/kodekloudrepos/demo`.  
Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:  
On Storage server in Stratos DC create a new branch `xfusioncorp_demo` from `master` branch in `/usr/src/kodekloudrepos/demo` git repo.  
Please do not try to make any changes in the code.

Solution:  
```
ssh natasha@ststor01
git branch -a
git switch master
git checkout -b xfusioncorp_demo
git push origin xfusioncorp_demo
```

[back](https://github.com/MederD)  

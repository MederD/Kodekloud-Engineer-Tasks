The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/ecommerce` present on Storage server in Stratos DC.  
This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree,  
so they want to point back the HEAD and the branch itself to a commit with message `add data.txt file`. Find below more details:  
In /usr/src/kodekloudrepos/ecommerce git repository, reset the git commit history so that there are only two commits in the commit history i.e `initial` commit and `add data.txt file`.  
Also make sure to push your changes.

Solution:  
```
ssh natasha@ststor01
sudo su
cd /usr/src/kodekloudrepos/ecommerce
git log ---> # find <commit-hash> for "add data.txt file"
git reset --hard <commit-hash>
git status
git push --force
git status
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

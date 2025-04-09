A couple of new Git repositories were created recently and this is still in progress. Recently a new requirement has emerged to create a repository on the storage server. Below are the details for this request.  
Create and initialize a git repository at `/home/sarah/story-blog-t1q4` on Storage server.  
Let's add a file named `lion-and-mouse-t1q4.txt` to our project inside `/home/sarah/story-blog-t1q4`, the file content must be `A Lion lay asleep in the forest`.  
Use below credentials to SSH into the storage server and to complete this task.  
Username: `sarah`  
Password: `S3cure321`  

Solution:  
```
ssh sarah@ststor01
mkdir -p /home/sarah/story-blog-t1q4 && cd $_
git init
echo "A Lion lay asleep in the forest" > lion-and-mouse-t1q4.txt
```
---
The Nautilus dev team is currently in the process of creating Git repositories, a new requirement has emerged to create a repository on the storage server. Below are the details for this request.  
Create and initialise a git repository at `/home/sarah/story-blog-t1q3` on Storage server.  

Solution:  
```
mkdir -p /home/sarah/story-blog-t1q3 && cd $_
git init
```
---
A couple of new Git repositories were created recently and this is still in progress. The developers now has started adding some content under these repositories. Below are the details for this request.  
There is a file named `lion-and-mouse-t1q5.txt` which is placed under `/home/sarah/story-blog-t1q5` repository, stage the same to make it available for commit.  

Solution:  
```
cd /home/sarah/story-blog-t1q5
git add lion-and-mouse-t1q5.txt
git status
```
---
The Nautilus application development team has been working on a project repository `/opt/apps-t2q3.git`. This repo is cloned at `/usr/src/kodekloudrepos/apps-t2q3` on storage server in Stratos DC. They recently shared the following requirements with DevOps team:  
Create a new branch `devops-t2q3` in `/usr/src/kodekloudrepos/apps-t2q3` repo from `master` and copy the `/tmp/index-t2q3.html` file (present on storage server itself) into the repo.  
Further, add/commit this file in the new branch and merge back that branch into master branch. Finally, push the changes to the origin for both of the branches.  

```
cd cd /usr/src/kodekloudrepos/apps-t2q3
git checkout -b devops-t2q3
cp /tmp/index-t2q3.html .
git add index-t2q3.html
git commit -m "Add index-t2q3.html file"
git checkout master
git merge devops-t2q3
git push origin master
git push origin devops-t2q3
```
---
Nautilus developers are actively working on one of the project repositories, `/usr/src/kodekloudrepos/apps-t2q1`. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch.  
Below are the requirements that have been shared with the DevOps team:  
On Storage server in Stratos DC create a new branch `xfusioncorp_apps-t2q1` from `master` branch in `/usr/src/kodekloudrepos/apps-t2q1` git repo.  
Please do not try to make any changes in the code.  

```
cd /usr/src/kodekloudrepos/apps-t2q1
git checkout master
git checkout -b xfusioncorp_apps-t2q1
git push origin xfusioncorp_apps-t2q1
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

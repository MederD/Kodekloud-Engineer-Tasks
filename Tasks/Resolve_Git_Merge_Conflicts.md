Sarah and Max were working on writting some stories which they have pushed to the repository. Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. Below you can find more details:  

SSH into storage server using user max and password Max_pass123. Under /home/max you will find the story-blog repository. Try to push the changes to the origin repo and fix the issues. The story-index.txt must have titles for all 4 stories. Additionally, there is a typo in The Lion and the Mooose line where Mooose should be Mouse.  

Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8000 and click on Display Port. You should be able to access the Gitea page. You can login to Gitea server from UI using username sarah and password Sarah_pass123 or username max and password Max_pass123.  

Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.  

Solution:  

1. ssh to storage server:  
```
ssh max@ststor01
```

2. pull the repo:  
```
git pull origin master
```

3. fix the typo and the differences.  
4. push back to branch  
```
git push origin master
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)



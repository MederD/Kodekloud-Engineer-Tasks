The Nautilus development team shared with the DevOps team requirements for new application development, setting up a Git repository for that project. Create a Git repository on Storage server in Stratos DC as per details given below:  

1.) Install git package using yum on Storage server.  

2.) After that create/init a git repository /opt/games.git (use the exact name as asked and make sure not to create a bare repository).  

Solution:  

1.) SSH to natasha@ststor01  
2.) Install git:  
```
sudo yum install git -y
```

3.) Create repo:  
```
sudo mkdir -p /opt/games.git && cd $_
```

4.) Initialize git:  
```
sudo git init
```

[back](https://github.com/MederD)  

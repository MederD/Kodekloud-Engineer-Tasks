The Nautilus development team shared requirements with the DevOps team regarding new application development.—specifically, they want to set up a Git repository for that project. Create a Git repository on Storage server in Stratos DC as per details given below:  

    Install git package using yum on Storage server.  

    After that create a bare repository /opt/beta.git (make sure to use exact name).  

Solution:  

1.) SSH to natasha@ststor01  
2.) Install git:  
```
sudo yum install git -y
```

3.) Create repo:  
```
sudo mkdir -p /opt/beta.git && cd $_  
```

4.) Initialize git:  
```
sudo git init --bare
```

[back](https://github.com/MederD)  

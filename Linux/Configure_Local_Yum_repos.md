The Nautilus production support team and security team had a meeting last month in which they decided to use local yum repositories for maintaing packages needed for their servers.  
For now they have decided to configure a local yum repo on Nautilus Backup Server. This is one of the pending items from last month, so please configure a local yum repository on Nautilus Backup Server as per details given below.  

a. We have some packages already present at location /packages/downloaded_rpms/ on Nautilus Backup Server.  

b. Create a yum repo named localyum and make sure to set Repository ID to localyum. Configure it to use package's location /packages/downloaded_rpms/.  

c. Install package vim-enhanced from this newly created repo.  

Solution:  
```bash
ssh clint@stbkp01
sudo su
vi /etc/yum.repos.d/localyum.repo
```  
then, add below to the localyum.repo:  
```txt
[localyum]
name=localyum
baseurl=file:///packages/downloaded_rpms/
gpgcheck=0
enabled=1
```

then:  
```bash
createrepo -v /packages/downloaded_rpms/
yum repolist
yum install --disablerepo="*" --enablerepo="localyum" vim-enhanced
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 


One of the Nautilus project developers need access to run docker commands on App Server 3. This user is already created on the server. Accomplish this task as per details given below:  
User anita is not able to run docker commands on App Server 3 in Stratos DC, make the required changes so that this user can run docker commands without sudo.  

Solution:  
```bash
ssh banner@stapp03
sudo usermod -aG docker anita
sudo systemctl restart docker
su - anita
docker ps -a
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)   

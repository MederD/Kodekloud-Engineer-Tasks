Some of the developers from Nautilus project team have asked for SFTP access to at least one of the app server in Stratos DC.  
After going through the requirements, the system admins team has decided to configure the SFTP server on App Server 2 server in Stratos Datacenter. Please configure it as per the following instructions:  
a. Create a SFTP user `siva` and set its password to `8FmzjvFU6S`. There is already a group called ftp, you can utilise the same.  
b. Password authentication should be enabled for this user.  
c. SFTP user should only be allowed to make SFTP connections.  

Solution:  
```
ssh steve@stapp02
sudo su
useradd -g ftp -d /ftp -s /sbin/nologin siva
echo 'siva:8FmzjvFU6S' | chpasswd
mkdir -p /ftp/siva/files
chown siva:ftp /ftp/siva/files
chmod 700 /ftp/siva/files
vi /etc/ssh/sshd_config
`
PasswordAuthentication yes
AllowGroups ftp sshd

Match Group ftp
  ChrootDirectory /ftp/%u
  ForceCommand internal-sftp
  AllowTcpForwarding no
  X11Forwarding no
`
systemctl reload sshd
```
exit back to jumphost and check:  
```
sftp siva@stapp02
siva@stapp02's password: 
Connected to stapp02.
sftp>
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

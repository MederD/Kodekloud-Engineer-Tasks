We have confidential data that needs to be transferred to a remote location, so we need to encrypt that data.We also need to decrypt data we received from a remote location in order to understand its content.  
On storage server in Stratos Datacenter we have private and public keys stored at /home/*_key.asc. Use these keys to perform the following actions.  
  
- Encrypt /home/encrypt_me.txt to /home/encrypted_me.asc.  
- Decrypt /home/decrypt_me.asc to /home/decrypted_me.txt. (Passphrase for decryption and encryption is kodekloud).  
- The user ID you can use is kodekloud@kodekloud.com.

Solution:  
```
ssh natasha@ststor01
sudo su
gpg --import /home/public_key.asc
gpg --import /home/private_key.asc
```
Encryption:  
```
gpg --output /home/encrypted_me.asc --encrypt --recipient kodekloud@kodekloud.com /home/encrypt_me.txt
```
Decryption:  
```
gpg --output /home/decrypted_me.txt --decrypt /home/decrypt_me.asc
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)  

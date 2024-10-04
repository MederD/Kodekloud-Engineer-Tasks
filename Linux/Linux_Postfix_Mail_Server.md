xFusionCorp Industries has planned to set up a common email server in Stork DC.  
After several meetings and recommendations they have decided to use postfix as their mail transfer agent and dovecot as an IMAP/POP3 server. We would like you to perform the following steps:  

1. Install and configure postfix on Stork DC mail server.  
2. Create an email account ```kirsty@stratos.xfusioncorp.com``` identified by ```ksH85UJjhb.```
3. Set its mail directory to /home/kirsty/Maildir.  
4. Install and configure dovecot on the same server.

Solution:  
```
ssh groot@stmail01
sudo su
yum install postfix -y
```

Uncomment/Edit/Add below configuration on ```/etc/postfix/main.conf```:   
```
myhostname = stmail01.stratos.xfusioncorp.com	
mydomain = stratos.xfusioncorp.com	
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
mynetworks = 172.16.238.0/24, 127.0.0.0/8
home_mailbox = Maildir/
# inet_interfaces = localhost
```

```
systemctl start postfix
systemctl status postfix
useradd kirsty
passwd kirsty

yum install dovecot -y
```
Uncomment/Edit/Add below configuration on ```/etc/dovecot.conf```:  
```
protocols = imap pop3 
```
Uncomment/Edit/Add below configuration on ```/etc/dovecot/conf.d/10-mail.conf```:  
```
mail_location = maildir:~/Maildir
```
Uncomment/Edit/Add below configuration on ```/etc/dovecot/conf.d/10-auth.conf```:  
```
disable_plaintext_auth = yes
auth_mechanisms = plain login
```
Uncomment/Edit/Add below configuration on ```/etc/dovecot/conf.d/10-master.conf```:  
```
service auth {
...
  unix_listener auth-userdb {
    mode = 0660
    user = postfix
    group = postfix
  }
  ...
}
```
```
systecmtl start dovecot
systemctl status dovecot
```
Optional postfix testing:   
```
telnet stmail01 25
...
Connected to stmail01.
Escape character is '^]'.
220 mail.stratos.xfusioncorp.com
EHLO localhost
...
250 mail.stratos.xfusioncorp.com
...
mail from:kirsty@stratos.xfusioncorp.com
250 2.1.0 Ok
rcpt to:kirsty@stratos.xfusioncorp.com
250 2.1.0 Ok
...
DATA
354 End data with <CR><LF>.<CR><LF>
This is a test email.
.
250 2.0.0 O
quit
```
then:  
```
su - kirsty
cat /home/kirsty/Maildir/new/172XXXXXXX
```

Optional dovecot testing:  
```
telnet stmail01 110
...
Connected to stmail01.
Escape character is '^]'.
+OK Dovecot ready.
user kirsty
+OK
pass ksH85UJjhb
+OK Logged in.
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 






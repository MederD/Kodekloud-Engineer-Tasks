The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC.  
After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:  
```
a. Install tomcat server on App Server 1.
b. Configure it to run on port 3004.
c. There is a ROOT.war file on Jump host at location /tmp.
```  
Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e `curl http://stapp01:3004`

Solution:
```
ssh tony@stapp01
sudo yum install -y tomcat tomcat-admin-webapps
sudo vi /etc/tomcat/server.xml
```
Change the following section:  
```
<Connector port="3004" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```
```
sudo systemctl restart tomcat
sudo systemctl status tomcat
exit
scp /tmp/ROOT.war user@stapp01:/tmp/
ssh tony@stapp01
sudo mv /tmp/ROOT.war /usr/share/tomcat/webapps/
sudo systemctl restart tomcat
sudo systemctl status tomcat
exit
curl http://stapp01:3004

```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 


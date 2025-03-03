The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC.  
The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:  
PostgreSQL database server is already installed on the Nautilus database server.  
a. Create a database user kodekloud_aim and set its password to GyQkFRVNr3.  
b. Create a database kodekloud_db1 and grant full permissions to user kodekloud_aim on this database.  
Note: Please do not try to restart PostgreSQL server service.  

Solution:  
```
ssh peter@stdb01
psql -U postgres
CREATE USER kodekloud_aim WITH PASSWORD 'GyQkFRVNr3';
CREATE DATABASE kodekloud_db1;
GRANT ALL PRIVILEGES ON DATABASE "kodekloud_db1 to kodekloud_aim;
\l
\du
```
[reference](https://neon.tech/postgresql/postgresql-administration/postgresql-list-users)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

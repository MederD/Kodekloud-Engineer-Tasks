The Nautilus DevOps team is working on to develop a bash script to automate some tasks. As per the requirements shared with the team database related tasks needed to be automated. Below you can find more details about the same:  
    Write a bash script named /opt/scripts/database.sh on Database Server. The mariadb database server is already installed on this server.  
    Add code in the script to perform some database related operations as per conditions given below:  
    a. Create a new database named kodekloud_db01. If this database already exists on the server then script should print a message Database already exists and if the database does not exist then create the same and script should print Database kodekloud_db01 has been created.  
    Further, create a user named kodekloud_roy and set its password to asdfgdsd, also give full access to this user on newly created database (remember to use wildcard host while creating the user).  
    b. Now check if the database (if it was already there) already contains some data (tables)if so then script should print 'database is not empty otherwise import the database dump /opt/db_backups/db.sql and print imported database dump into kodekloud_db01 database.  
    c. Take a mysql dump which should be named as kodekloud_db01.sql and save it under /opt/db_backups/ directory.  

Solution:  
```
ssh peter@stdb01
sudo vi /opt/scripts/database.sh
```
This is the script:  
```
#!/bin/bash

DB_NAME="kodekloud_db01"
DB_USER="kodekloud_roy"
DB_PASSWORD="asdfgdsd"
DB_DUMP="/opt/db_backups/db.sql"
BACKUP_DIR="/opt/db_backups"
DUMP_NAME="kodekloud_db01.sql"

DB_EXISTS=$(mysql -u root -e "SHOW DATABASES LIKE '${DB_NAME}'" | grep "${DB_NAME}")

if [ -z "$DB_EXISTS" ]; then
    mysql -u root -e "CREATE DATABASE ${DB_NAME};"
    echo "Database ${DB_NAME} has been created"

    # Create the user and grant full access to the database
    mysql -u root -e "CREATE USER '${DB_USER}'@'%' IDENTIFIED BY '${DB_PASSWORD}';"
    mysql -u root -e "GRANT ALL PRIVILEGES ON ${DB_NAME}.* TO '${DB_USER}'@'%';"
    mysql -u root -e "FLUSH PRIVILEGES;"

else
    echo "Database already exists"
fi

TABLE_EXISTS=$(mysql -u root -e "USE ${DB_NAME}; SHOW TABLES;" | grep -q .)

if [ "$TABLE_EXISTS" ]; then
    echo "database is not empty"
else
    if [ -f "$DB_DUMP" ]; then
        mysql -u root ${DB_NAME} < $DB_DUMP
        echo "imported database dump into ${DB_NAME} database."
    fi
fi

mysqldump -u root ${DB_NAME} > "${BACKUP_DIR}/${DUMP_NAME}"
```
```
sudo chmod +x /opt/scripts/database.sh
sudo /opt/scripts/database.sh
```
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks) 

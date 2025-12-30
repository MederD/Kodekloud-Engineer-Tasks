The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to Azure. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.  
Recently, they started working on creating and configuring some database instances on Azure.  
For this task, create one publicly accessible Azure SQL Database instance along with the following details:  
1) The name of the Azure SQL Database must be devops-sqldb.  
2) The server name must be devops-server-14677.  
3) The compute + storage configuration should be Basic (For less demanding workloads).  
4) The backup storage redundancy should be Locally-redundant backup storage.  
5) Set the login admin username to devops-admin and set an appropriate password.  
6) Set the database size to 2 GiB.  
7) Keep the rest of the configurations as default. Finally, make sure the database is in the Ready state before submitting this task.

Solution:  
```bash
rg=$(az group list --query "[].name" -o tsv)
az sql server create -n devops-server-14677 -g $rg --admin-user devops-admin --admin-password 'Password123' --enable-public-network true
az sql db create -g $rg -n devops-sqldb -s devops-server-14677 --backup-storage-redundancy local --edition Basic
az sql db list -g $rg -s devops-server-14677 --query "[].name"
az sql server firewall-rule create -g $rg -s devops-server-14677 -n AllowAll --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
telnet devops-server-14677.database.windows.net 1433
```

[reference](https://learn.microsoft.com/en-us/azure/azure-sql/database/scripts/create-and-configure-database-cli?view=azuresql)  
[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)

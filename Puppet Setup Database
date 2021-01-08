**1 - On Jump Host will create file with given name.**
    
node 'stdb01.stratos.xfusioncorp.com' {
include mysql_database
}

class mysql_database {
    package {'mariadb-server':
        ensure => installed,
    }
  
    service { 'mariadb':
        ensure => running,
        enable => true,
  
    }
    
    mysql::db { 'database_name':
    user => 'user_name',
    password => 'user_password',
    host => 'localhost',
    grant => ['ALL'],
    }
}

**2 - on agent node**
     sudo puppet agent -t
 then, check the maridb status ---> sudo systemctl status maridb
 if running login to mysql ---> mysql -u user_name -p

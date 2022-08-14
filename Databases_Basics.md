#DataBases

Types
- SQL (Relational Storage of Information)
- NO-SQL (Storage is document based or key-value store)

## MySQL

Open-source Database. Community (Free) and Commercial Edition
1. Wget community release
2. RPM -ivh ...
3. yum install mysql-server
4. service mysqld start
5. Logs `var/log/mysqld.log`
6. Default port : 3306
7. Connect to database using temporary password `mysql -u root -p{password}` (give password in prompt)
8. `ALTER USER 'root@localhost' IDENTIFIED BY 'NewPassword'` or mysql_secure_installation
9. `SHOW DATABASES`
10. `CREATE DATABASE school`;
11. `USE school`;
12. `SHOW TABLES`

### Issues in MYSQL
1. Cration user with restricted permissions
  - `CREATE USER 'john'@'localhost' IDENTIFIED by 'password'`
  - localhost is system to which they are connected. John can connect to the MYSQL from localhost ONLY not from any other remote server
  - 'john@192.168.1.0' will allow john to connect from remote server
  - 'john@%' for all server connect
2. Privileges
  - `GRANT <permission> ON >db.table> TO 'john'@'%'`
  -  `SHOW GRANTS for 'john'@'localhost' `
3. Install MYSQL-DB
  - `sudo yum install mariadb-server` OR
  - `sudo yum install https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm`
  - `sudo yum install mysql-community-server`
  - `sudo systemctl start mariadb.service` or `sudo systemctl start mysqld.service`

### Config

1. DB Installed is Maria db
2. Password for root is set to 'rakshit'
3. Config file : `/etc/my.cnf` 



##MongoDB

1. Logs are `var/log/mongodb/mongod.log`
2. Default port : 27017 , listening on  localhost
3. Conf file : `/etc/mongod.conf`
4. `mongo` to connect to the Shell
5. https://linuxize.com/post/how-to-install-mongodb-on-centos-7/

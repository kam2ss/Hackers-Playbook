##### Enumerate Empty Password/Anonymous Login
```
nmap -sV -p3306 --script mysql-empty-password
```

##### Check if `InteractiveClient` Capability is supported on the MySQL
```
nmap -p3306 --script mysql-info IP
```

##### `mysql-users` Nmap Scripts
```
nmap -p3306 --script mysql-users --script-args="mysqluser='root',mysqlpass=''" IP
```

##### List all Databases Stored on the MySQL
```
nmap -p3306 --script mysql-databases --script-args="mysqluser='root',mysqlpass=''" IP
```

##### Find the Data Directory used by MySQL
```
nmap -p3306 --script mysql-users --script-args="mysqluser='root',mysqlpass=''" IP | grep datadir
```

##### Check whether File Privileges can be Granted to Non Admin Users
```
nmap -p3306 --script mysql-audit --script-args="mysql-audit.username='root',mysql-audit.password=",mysql-audit.filename="/usr/share/nmap/nselib/data/mysql-cis.audit" IP
```

##### Dump all User Hashes
```
nmap -p3306 --script mysql-users --script-args=mysqluser='root',mysqlpass=''" IP
```

##### Find the number of Records Stored in Table 'authors' in Database 'books'
```
nmap --script mysql-query --script-args="query='select count(*) from books.authors;',username='root',password=''" -p 3306 192.71.145.3
```
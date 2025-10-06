#### Metasploit
```
use auxiliary/scanner/ftp/ftp_login
set rhosts <IP>
set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt
set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
run
```

#### Check Anonymous logon
```
use auxiliary/scanner/ftp/anonymous
set rhosts <ip>
run
```


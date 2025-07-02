##### Metasploit Module to find the support for SMB2 in Samba Server
```
msfconsole
use auxiliary/scanner/smb/smb2
set rhosts IP
run
```

##### List all users on the Samba using smb_enumusers
```
search smb_enumusers
use auxiliary/scanner/smb/smb_enumusers
options
set rhosts IP
run
```

##### List all available shares on the Samba using smb_enumshares
```
use auxiliary/scanner/smb/smb_enumshares
set rhosts IP
run
```

##### If the ***Null Login*** is not allowed we can try the password ***Brute Force***.
```
search smb_login
use auxiliary/scanner/smb/smb_login
set rhosts IP
set pass_file /usr/share/wordlists/metasploit/unix_passwords.txt
set smbuser jane
run
```

##### List Named Pipes available over SMB on the Samba Server 
Named Pipes are a way for inter-process communication on the same machine, without involving the network stack.
```
serach pipe_auditor
use auxiliary/scanner/smb/pipe_auditor
set smbuser admin
set smbpass password1
set rhosts IP
run
```
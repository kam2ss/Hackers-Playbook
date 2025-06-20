#### What is the Active Directory (AD) Environment?
It is a Windows-based directory service that ***stores and provides data objects to the internal network*** environment. It allows for ***centralized management of authentication and authorization***.

**Find if the Windows machine is part of the AD environment or not:**
```
systeminfo | findstr Domain
```
**Note: If we get WORKGROUP in the domain section, then it means that this machine is part of local workgroup.**

#### Enumerate Users & Groups
**PowerShell**
***Account discovery is the first step*** once we have gained initial access to the compromised machine:
```
Get-ADUser -Filter *
```

**LDAP Hierarchical Tree Structure**
We can use this to find a user within the AD environment. The `Distinguished Name (DN)` is a ***collection of comma-seperated key and value pairs used to identify unique records within the directory***. The DN consist of `Domain Component (DC), Organizational Unit (OU), Common Name (CN)`, and others. [[AD Theory]]
	**Example of DN:**
		`"CN=User1,CN=Users,DC=thmredteam,DC=com"`

Using the `SearchBase` option, we specify Common-Name `CN` in the active directory. 
```
Get-ADUser -Filter * -SearchBase "CN=Users,DC=THMREDTEAM,DC=COM"
```

List available user accounts within `thm` OU in the `theredteam.com` domain
```
Get-ADUser -Filter * -SearchBase "OU=THM,DC=THMREDTEAM,DC=COM"
```



A password spraying attack targets many usernames using one common weak password, which could help avoid an account lockout policy.

To be successful in the password spraying attack, we need to enumerate the target and create a list of valid usernames (or email addresses list).

Next, we'll apply the password spraying technique using different scenario:
- SSH
- RDP
- Outlook web access (OWA) portal
- SMB

Assuming that we have already enumerated the system and created a valid username list.
#### SSH
Write a Python script [[Season, Year, Special Character]] to generate a password list.
```
# Using a single password
hydra -L username.txt -p PASSWORD ssh://IP

# Using a password list
hydra -L username.txt -P password.txt ssh://IP
```

#### RDP
We can use `RDPassSpray` tool to password spray against RDP.

**Single Host**
```
python3 RDPassSpray.py -u victim -p Spring2021! -t IP:3026
```

**Active Directory Environment**
```
python3 RDPassSpray.py -U username-list -p Spring2021! -d THM-labs -T RDP_SERVERS-LIST
```

#### Outlook Web Access (OWA) Portal
Tools:
- [SprayingToolkit](https://github.com/byt3bl33d3r/SprayingToolkit) (atomizer.py)
- [MailSniper](https://github.com/dafthack/MailSniper)

#### SMB
Tool:
- Metasploit (auxiliary/scanner/smb/smb_login)


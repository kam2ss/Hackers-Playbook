##### Once the port 445 is revealed from the initial scan, we can perform further SMB scans.
```
nmap IP
```

### Using a Guest User
##### To list supported protocols and dialects of an SMB server
```
nmap -p445 --script smb-protocols IP
```

##### Running security mode script to return the info about the SMB security level
```
nmap -p445 --script smb-security-mode IP
```

##### Enumerating all available shares
**About IPC$ share**: 
The IPC$ share is also known as a ***null session*** connection. By using this session, Windows lets anonymous users perform certain activities, such as enumerating the names of domain accounts and network shares.
```
nmap -p445 --script smb-enum-shares IP
```

### Using an Admin credentials
##### Enumerating the users logged into a system through an SMB share
```
nmap -p445 --script smb-enum-sessions IP
nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=password1 IP
```

##### Scanning all shares using valid credentials to check the permissions
```
nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=password1 IP
```

##### Enumerate the windows users on a target machine
```
nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=password1 IP
```

##### Get info about the Server Statistics. It uses port 445 and 139 to fetch details
```
nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=password1 IP
```

##### Enumerating available Domains 
```
nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=password1 IP
```

##### Enumerating Available User Groups 
```
nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=password1 IP
```

##### Enumerating Services
```
nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=password1 IP
```

##### Enumerating all the shared Folders and Drives then running the 'ls' command.
```
nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=password1 IP
```
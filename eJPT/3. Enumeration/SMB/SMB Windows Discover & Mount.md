The ***Server Message Block (SMB)*** is a network protocol that ***enables file and resource sharing between computers on a network.***

***SMB*** can also be used to ***share files between computers that are not on the same local network***. However, this typically requires more complex network configuration, such as setting up a ***VPN or using DirectAccess***. It's not typically recommended due to ***potential security risks and performance issues***. Other methods like FTP, SFTP, or cloud-based solutions are often used.

Sets up the session for SMB.
```
PORT SERVICE
139  netbios-ssn
```

### SMB Recon with PowerShell
#### Checking IP address
```
ipconfig
Get-NetIPAddress
```

#### Nmap against the Subnet
```
nmap 10.0.24.0/20 --open
```

#### Mount the Target Machine Shared Folders/Map Network Drive
##### Using GUI
```
1. Right click on Network and select 'Map Network Drive'
2. Type target machine IP address '\\IP' in Folder: field and hit Browse 
3. Once the network share is discovered, input the username:password
4. Select the drive and click 'Ok' and finish
```

##### Using Cmd Prompt
```
# Clear the stored session
net use * /delete

# Mount the target folder
net use Z: \\IP\C$ smbserver_771/USER:administrator # -> passwd/username
```
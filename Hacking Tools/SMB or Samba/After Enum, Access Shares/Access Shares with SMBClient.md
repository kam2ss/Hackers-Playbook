#### Enumerate SMB shares as the user over SMBv3
```
smbclient -U <USERNAME> -L <server> -m SMB3
```

#### Access to the Share without Authentication
```
smbclient -L //IP/Share$ -N
```
- `-N:` Anonymous

```
smbclient //IP/Share -m SMB2 -N
```
#### Access the server C$ share as the user
```
smbclient -U <USERNAME> //<server>/C$ -m SMB3
```

#### Access the server C$ share as the DOM domain user
```
smbclient -U DOM\\<USERNAME> //<server>/C$ -m SMB3
```



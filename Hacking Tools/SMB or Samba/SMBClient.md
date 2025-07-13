#### Anonymous access
```bash
smbclient -L //<target> -N
```
- `-L:` Lists all the shares
- `-N:` Tells **smbclient** not to prompt for a password.

**If the above command doesn't list the share that can be accessed with anonymous login. Then use the brute-force script**
```bash
#!/bin/bash

TARGET_IP="target"  # replace with your target IP

while read -r share; do
    # Try to connect but hide all output
    smbclient "\\\\$TARGET_IP\\$share" -N -c "quit" &>/dev/null
    if [ $? -eq 0 ]; then
        echo "Anonymous access allowed on: $share"
    fi
done < shares.txt  # replace with your wordlist file
```

#### Access the Share
```bash
smbclient \\\\<target>\\<share> -N
```

---
#### Authenticated user:
```bash
smbclient -L //10.10.10.5 -U user1
```
- It will prompt for a password.

---
#### Specific domain (Windows AD):
```bash
smbclient -L //10.10.10.5 -U 'DOMAIN\\user1'
```

---
#### Connect directly to a known share:
```bash
smbclient //10.10.10.5/shared_folder -N
```
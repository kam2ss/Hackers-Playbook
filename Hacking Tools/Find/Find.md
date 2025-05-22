#### Find file with SUID bit set
```
find / -perm -4000 -type f 2>/dev/null
find / -name sudo -type f -executable 2>/dev/null 
```

#### Find files with SGID bit set
```
find / -perm -2000 -type f 2>/dev/null
```

#### Find world-writeable files owned by root
```
find / -type f -writable -user root 2>/dev/null
```

#### Find Cron Jobs Owned by root
```
find /etc/cron* -type f -user root 2>/dev/null
```

#### Find Binaries with Special Capabilities
```
sudo getcap -r / 2>/dev/null
```

---
#### With Known Username
```
find / -user USERNAME -exec grep -a -i "password" {} 2>/dev/null \;
```

```
find / -path "*/.*" -type f -exec grep -a -i "password" {} 2>/dev/null \;
```


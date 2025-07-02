When the all the TCP ports are closed, we can perform an UDP scan.
```
# '-Pn' skips host discovery, '-F':scans 100 most commonly used ports
nmap -Pn -F IP
```
When the all the TCP ports are closed, we can perform an UDP scan.
```
# Basic UDP scan 
nmap -sU -p 1-250 IP

# To perform a UDP and version scan
nmap -sUV -p 134,177,234 IP

# If the version is not revealed, run the nmap script
nmap -p134 -sUV --script=discovery IP

# To confirm the port 134 is TFTP
tftp IP 134
```
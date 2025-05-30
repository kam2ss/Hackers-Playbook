### Live Host Discovery
**Listing the Targets without Scanning them**
```
nmap -sL <target_ip>
```

**Provided a list of hosts**
```
nmap -iL <list_of_hosts.txt>
```

list of hosts that Nmap will scan: nmap -sL TARGETS. (If you don’t want Nmap to the DNS server, you can add -n.)

| **Scan Type**              | **Example Command**                       |
| -------------------------- | ----------------------------------------- |
| ARP Scan                   | sudo nmap -PR -sn MACHINE_IP/24           |
| ICMP Echo Scan             | sudo nmap -PE -sn MACHINE_IP/24           |
| ICMP Timestamp Scan        | sudo nmap -PP -sn MACHINE_IP/24           |
| ICMP Address Mask Scan     | sudo nmap -PM -sn MACHINE_IP/24           |
| TCP SYN Ping Scan          | sudo nmap -PS22,80,443 -sn MACHINE_IP/30  |
| TCP ACK Ping Scan          | sudo nmap -PA22,80,443 -sn MACHINE_IP/30  |
| UDP Ping Scan              | sudo nmap -PU53,161,162 -sn MACHINE_IP/30 |
| Scan without scanning them | Nmap -sL TARGET                           |

Remember to add -sn if you are only interested in host discovery without port-scanning. Omitting -sn will let Nmap default to port-scanning the live hosts.

| Option | Purpose                          |
| ------ | -------------------------------- |
| -n     | no DNS lookup                    |
| -R     | reverse-DNS lookup for all hosts |
| -sn    | host discovery only              |

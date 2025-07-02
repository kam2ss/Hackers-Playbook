While doing the port scan, capture the packets in `Wireshark` and analyze it. 

##### Check if the port is filtered
```
# Scan an IP address and check if the ports says 'filtered'
nmap -Pn -sS -F IP

# Take an open port and scan it to confirm
nmap -Pn -sA -p445,139 IP
```

##### Fragment the Packets
```
# lowercase '-f' to fragment the nmap packets
nmap -Pn -sS -sV -f --mtu 32 IP
```

##### Decoy the IP
```
# Using the router as a Decoy. Multiple IPs can be used
nmap -f --data-length 200 -D 10.10.10.1 IP
```

##### Change the Source Port 
```
# Make it look like it's coming from port 53, use '-g'
nmap -f --data-length 200 -g 53 -D 10.10.10.1 IP
```

##### Delay the Scan Probe
```
# Stealthier option by delaying the every few seconds
nmap -sS -sV --scan-delay 15s IP
```

##### Timing Template
```
# Timing templates are better then '--scan-delay'
nmap -sS -sV -T1 IP
```

##### Output Formats
```
# To import the XML results to msfconsole, postgresql needs to be started
Start the postgresql 

```
### Network scanning
Network scanning is an information-gathering technique used to identify key information about a computer network. It can be further broken down into two activities: ***network port scanning*** and ***vulnerability scanning***.

**Port scanning** attempts to identify live (or active) hosts on a network by using features of the network protocol to signal devices and await a response. The characteristics of this response can determine if the targeted host is a live host and which ports are open. Other types of response can indicate if the host is situated behind a filter, such as a firewall.

**Vulnerability scanning** is subsequently used to identify if any known vulnerabilities exist for each of the services running on the identified open ports. Vulnerability scans check for known vulnerable operating systems or software versions, known bad configurations, weak and default passwords, and denial of service flaws.

Network scanning activities are usually conducted to assess the current network security posture as part of a penetration test. However, when done by an anonymous individual without express permission, this can be the prelude to an attack.

### Netcat and Telnet
Simple tools that exist natively on most Linux systems, such as Netcat or Telnet, can be used as basic port scanners. They lack the sophistication of advanced tools like Nmap but can still prove valuable in a pinch.

To scan a network with Netcat we can use the following command which will initiate a scan of the first 1000 ports on the target IP address. The -z option instructs Netcat to perform a port scan instead of attempting to make a connection. The -v option tells Netcat to provide more verbose output.
```bash
# Show help page

nc -h

# Run a scan against the first 1000 ports

nc -z -v 127.17.0.2 1-1000
```

Netcat can also be used for banner grabbing, transferring data between a server and a client, and connecting to remote shells. Check out these labs for more information:

### Nmap
Nmap has several scanning modes designed for different situations – even for stealth scanning. The most common is SYN scanning, which sends the first stage of the TCP three-way handshake (the SYN packet) to determine live hosts in a range. If the host responds with the synchronisation acknowledged (SYN/ACK) packet, then the port is open.

```bash
# Get the Nmap help pagesnmap -h# Run a basic SYN scan of a designated targetnmap -sS 172.17.0.2
```


More advanced tools, such as Nessus, have the ability to scan for known vulnerabilities and misconfigurations in addition to live hosts and open ports when scanning a network. Nessus's scanning engine covers a range of technologies, including operating systems, network devices, databases and web servers.



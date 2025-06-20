### Background
Both Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) scanning are important, yet UDP is especially so as security for these ports is often overlooked by the admins configuring them and any inexperienced cyber professionals testing them.

### TCP vs UDP
The difference between TCP and UDP is that the former is a stateful protocol, able to keep track of a connection, whereas the latter is a stateless protocol, which can behave in any way based on the application layer protocol. 

### TCP Scanning
TCP scanning is most common because the usual services run over TCP. 

- **-sS (SYN Scan)** – Synchronise (SYN) scanning generally requires **root** privileges because it needs the capability to create and alter raw packets. However it's faster than the following scans (as it doesn't complete the TCP handshake) and quite accurate in determining the port states.
    - **Open** – A valid Synchronise (SYN)/Acknowledge (ACK) is received
    - **Closed** – A Reset (RST) is received
    - **Open | filtered** – Timeout or Internet Control Message Protocol (ICMP) unreachable is received
    
- **-sT (Connect Scan)** – Connect scanning should be used in cases where SYN scanning is not available. Not only is it slower (as it must complete the whole handshake), but basic systems are more likely to log the scan attempt. It's true that IDS products will log both, but in many environments they aren't so common.

- **-sN, -sF, -sX (NULL, FIN and XMAS Scans) –** These types of scans all behave the same with the exception of the TCP flag bits set on the packets they send.
    - **-sN** – Sets no bits, i.e. TCP flag header is set to 0
    - **-sF** – Only sets the TCP FIN bit
    - -**sX** – Sets the FIN, PSH, and URG flags, lighting the packet up like a Christmas tree. The advantage of these scans is that they can sneak through some non-stateful firewalls, yet the downside is that they cannot correctly differentiate between open and filtered ports. If an RST is received the port is considered **closed**. If an ICMP unreachable is received the port is considered **filtered**. If no response is received the port is considered **open | filtered**, because an open port would not reply to those flags set.

Nmap offers several other scan types and even a **--scanflags** option for advanced users to set exactly what bits they want in the packets. 

### UDP Scanning
Unlike TCP scanning where protocols follow a certain set of rules, anything can happen with UDP scanning. It's generally slower to scan because Nmap waits for a response, but there's no guarantee it will come. Versioning (**-sV**) can be used to attempt port-specific payloads, but in general scans behave in the following way:

- **closed** – ICMP unreachable is received
- **filtered** – Other ICMP error is received
- **open | filtered** – The port is either open but the service did not send a response for the sent packet, or the port is filtered and the response was not received
- **open** – For the most common UDP ports, Nmap will send service-specific packets and mark them as open if a valid response is received

Depending on the purpose of the scan, there are host discovery options that can make it faster or more reliable. The following are some options that can help in most engagements:

- **-sn** (Ping Scan) – This is useful when you're facing large networks and need to quickly get a list of hosts to start enumerating. In this type of scan, Nmap uses ICMP/ARP (depending on the level of privilege Nmap started with) to assess whether or not a host is up and then completely skips port scanning.
- **-Pn** (No ping) – By default Nmap will try to ping the host to assess whether or not it is up. However there are cases when hosts (such as Windows-based hosts) do not reply to ICMP ping probes. We can suppress ping scanning with this option, making the scan more reliable but slower.
- **-n** (No DNS Resolution) – Nmap will always try to perform reverse DNS lookups on active hosts. Since DNS is rather slow, this option can be used to suppress DNS resolution.

## Scripting engine
Nmap's scripting engine is one of the most powerful tools in its arsenal. This lab focuses on existing scripts for well-known services.

- **-sC** (equivalent to --script=default) – This loads the default scripts for particular services and runs them against target ports to gain as much information about them as possible. It's the easiest option to use and is particularly helpful against UDP services as it sends port-specific packets and helps with assessing the state of the port.

Specific scripts can also be used, depending on the target port/service. The syntax for this is:
`--script <filename>|<category>|<directory>/|<expression>`

You must specify script arguments (comma separated) for some scripts. This can be done using the `--script-args` option.
```
# Demo
nmap --script snmp-sysdescr --script-args creds.snmp=admin <target>

# SMB Enumeration
nmap -p445 --script smb-enum-users --script-args smbusername=<username>,smbpass=<password> ip
```

For a specific service you can choose to run all available scripts. For example, the command `nmap -sT --script "http* and not(brute or dos)" <target>` will load all scripts that have names starting with ‘http’ and ignore those in the brute force and DoS category.

### Other useful options
Nmap has many capabilities, not all of which are listed here. Below are some of the common useful flags that can help with enumeration:
	
- **-iL** – A text file containing a newline-separated list of hosts to be scanned
- **--script-help** – Shows a description of what each selected script does
- **-O** – Based on response parameters, Nmap can try to guess the underlying OS of the target host
- **-sV** – Based on the responses and service-specific tests, Nmap can detect the service and version running on the target port
- **-p** – This flag specifies which ports are to be scanned. Valid parameters are:
    - one or more (comma separated) ports
    - a range of ports (dash-separated)
    - a dash (-) which represents the full port range (0-65535)
- **--top-ports <x>** – Scans the x top ports based on usage and popularity



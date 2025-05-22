### SNMP
**Simple Network Management Protocol** (SNMP) is a ***set of protocols used for network management and monitoring***. It is supported by a range of network devices, such as ***routers, switches, hubs, servers*** and ***workstations***. 

SNMP can not only ***get information*** on networked devices but also ***set/edit information on devices and change their behaviour***.

#### MIBs and OID
A **Management Information Base** (MIB) is a ***collection of information for managing network elements***. It contains ***managed objects*** like system name, CPU usage, interface status etc identified by an Object Identifier (OID). 

An **Object Identifier** (OID) ***consist of a series of numbers or strings separated by dots***, and they can query specific information. 

**Query all OIDs with `snmpwalk` for Port 161 (Default)**
```shell
snmpwalk <Target> -c <community> -v <version> .
```

**Query all OIDs with `snmpwalk` for Port 16161**
```
snmpwalk <Target>:<Port> -c <community> -v <version> .
```
### Exploiting SNMP
SNMP uses ***community strings*** as a ***user-id or password that allows access to a device's statistics***. If the community string provided is correct, the information requested will be returned.

SNMP community strings can be assigned different permissions. 
- **Read-Only (ro)**: enable a remote device to retrieve ‘***read-only***’ information from a device.
- **Read-Write (rw)**: allow remote devices to ***modify settings and content on that device***.

There are several ways to query devices managed under SNMP, a notable one being the ***Net-SNMP toolkit***. This includes commands such as ***snmpwalk***, which allows you to ***access lots of information at once***, and ***snmpset***, which can ***perform write operations***.

**Brute Forcing the Community String on Port 161 (Default)**
```shell
onesixtyone -c /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings.txt TARGET_IP
```

**Brute Forcing the Community String on Alternative Port**
```bash
onesixtyone -c /usr/share/doc/onesixtyone/dict.txt <IP> -p 16161
```

**snmpwalk - Read SNMP Data**
```shell
# snmpwalk -applicationoptions -commonoptions -oid target_ip
snmpwalk -v 2c -c public IP .
```
- `.`: means start walking from the root of the SNMP MIB tree (same as 1)

**snmpset - Modify SNMP Data**
```shell
# snmpset -commonoptions target_ip oid type value
snmpset -v 2c TARGET_IP oid type value
```

### Other Brute Forcing Tools
```shell
msf> use auxiliary/scanner/snmp/snmp_login
nmap -sU --script snmp-brute <target> --script-args snmp-brute.communitiesdb=<wordlist>
onesixtyone -c /usr/share/metasploit-framework/data/wordlists/snmp_default_pass.txt <IP>
hydra -P /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings.txt target.com snmp
```
Host discovery is the process of identifying ports, services, and operating systems used by a host on a particular network.

### ARP Sweep
The ARP Sweep module allows you to enumerate live hosts within the local network using **Address Resolution Protocol (ARP)** requests. 

Load the module with the `use` command.
```plaintext
msf > use auxiliary/scanner/discovery/arp_sweep
```

You can now reveal the module settings by typing `show options`.
The scan will run faster if you increase the number of threads in the `THREADS` field. It's recommended that this value be kept below 256 for Unix systems and under 16 for Win32 systems.

To set these fields to our intended target scan, you run the following commands:
```plaintext
msf auxiliary(arp_sweep) > set RHOSTS [IP]
RHOSTS => [IP]
msf auxiliary(arp_sweep) > set THREADS [number of threads]
THREADS => [number of threads]
msf auxiliary(arp_sweep) > run
```

### Port scanning
Once you have established which IPs are active, you can use a port scanner to discover if they have any open ports.
```
msf6 > search portscan
Matching Modules
================
#  Name                                              Disclosure Date  Rank    Check  Description
   -  ----                                              ---------------  ----    -----  -----------
   0  auxiliary/scanner/portscan/ftpbounce                               normal  No     FTP Bounce Port Scanner
   1  auxiliary/scanner/natpmp/natpmp_portscan                           normal  No     NAT-PMP External Port Scanner
   2  auxiliary/scanner/sap/sap_router_portscanner                       normal  No     SAPRouter Port Scanner
   3  auxiliary/scanner/portscan/xmas                                    normal  No     TCP "XMas" Port Scanner
   4  auxiliary/scanner/portscan/ack                                     normal  No     TCP ACK Firewall Scanner
   5  auxiliary/scanner/portscan/tcp                                     normal  No     TCP Port Scanner
   6  auxiliary/scanner/portscan/syn                                     normal  No     TCP SYN Port Scanner
   7  auxiliary/scanner/http/wordpress_pingback_access                   normal  No     Wordpress Pingback Locator
```

#### SYN scan
For this example, you will use the `auxiliary/scanner/portscan/syn` module. Attackers use a SYN scan to determine the state of a port without establishing a full connection. This is a “quiet” method of scanning for vulnerabilities that firewalls cannot trace, as it does not complete the connection to the server.  

When you have found which ports are open, you can also set the `RPORT` field to target a specific port (or ports) of an IP address. To set a range of ports, you would follow the format:

```plaintext
msf > set PORTS 12, 333, 44, 555
```

### Db Nmap
The `db_nmap` command runs Nmap and stores the results of the scan on a database on our machine.

To run this command, you first need to set up a database to store our project data. Metasploit uses PostgreSQL to do this. To start the service, you need to run the command:
```plaintext
service postgresql start
```
(Please note that other guides may use the `systemctl` command to set up the database, but this method is not available in our lab.)

Once PostgreSQL has been started, you need to initialize the data with `msfdb init`.
```plaintext
msfdb init
```

After you have initialized our msfconsole database, you can start Metasploit by running the command `msfconsole`.

To verify that Metasploit is connected to the database correctly, you can use the command `db_status` to verify the connection.
```plaintext
msf > db_status
```

![Metasploit confirmation that you are connected to msf with type postgresql.](https://il-labforge-assets.origin.immersivelabs.team/uploads/GGs4_7o4oSvwIHmTsYfKuncTUY9Qs3hJ0sYAfLv8uCo.png)

Now that our database is set up and the connection is verified, you can run the command:

```plaintext
msf > db_nmap -sV [Ip Address] -Pn
```

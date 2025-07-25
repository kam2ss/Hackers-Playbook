### Intro
_Lookup_ features real-world vulnerabilities like web app flaws and privilege escalation. It teaches key skills such as reconnaissance, enumeration, command injection, and scripting for automation—offering valuable experience in penetration testing and secure coding practices.
#### Nmap
```bash
nmap -p- -A 10.10.248.120
Starting Nmap 7.80 ( https://nmap.org ) at 2025-07-14 22:49 BST
Nmap scan report for ip-10-10-248-120.eu-west-1.compute.internal (10.10.248.120)
Host is up (0.00021s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Did not follow redirect to http://lookup.thm
MAC Address: 02:55:1A:4A:B6:E7 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=7/14%OT=22%CT=1%CU=41006%PV=Y%DS=1%DC=D%G=Y%M=02551A%T
OS:M=68757B71%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10A%TI=Z%CI=Z%II=I
OS:%TS=A)OPS(O1=M2301ST11NW7%O2=M2301ST11NW7%O3=M2301NNT11NW7%O4=M2301ST11N
OS:W7%O5=M2301ST11NW7%O6=M2301ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F
OS:4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M2301NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T
OS:=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R
OS:%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=
OS:40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0
OS:%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R
OS:=Y%DFI=N%T=40%CD=S)

Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.21 ms ip-10-10-248-120.eu-west-1.compute.internal (10.10.248.120)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.87 seconds
```

Browsing the IP gives us `We can’t connect to the server at lookup.thm.`, meaning the computer tried to reach the address `http://lookup.thm` but couldn't find or connect to the server. To overcome this issue we need to map the `ip` to `hostname` in `/etc/hosts (a local file where you manually map hostnames to IP addresses)` file because the internal lab domains aren't publicly registered. 

#### Browse the hostname
```bash
lookup.thm
```
- We get a basic login page.

When we try to input some random creds, we get an error saying `Wrong username or password. Please try again. Redirecting in 3 seconds.` However, when we try default creds like `admin:admin`, we only see the password error and not the username error `Wrong password. Please try again. Redirecting in 3 seconds.` which means that the `admin` is a genuine username.

Next we check the `Network` tab in `Inspect page/Web Developer Tools`.  We can see there's an error saying `Wrong username or password. Please try again. Redirecting in 3 seconds.` We can also see that the `HTTP Method: POST` and `Subdirectory: login.php` popped up for a few second. We can confirm this by nmap scan.

#### Nmap to Discover the Login Page
```bash
nmap -p80 --script http-enum <IP>
```

Now that we have managed to find a login page, we can try to `Brute-Force` this page.

#### Brute Force the Login Page with Hydra

#### Description
Hello there,  
  
I am the CEO and one of the co-founders of futurevera.thm. In Futurevera, we believe that the future is in space. We do a lot of space research and write blogs about it. We used to help students with space questions, but we are rebuilding our support.  
Recently blackhat hackers approached us saying they could takeover and are asking us for a big ransom. Please help us to find what they can takeover.  
Our website is located at [https://futurevera.thm](https://futurevera.thm/)
Hint: Don't forget to add the IP in /etc/hosts for futurevera.thm ; )

#### Adding the IP address in the `/etc/hosts` file
```bash
sudo vim /etc/hosts
<IP> futurevera.thm
```

### Enumeration
#### Nmap Scan
```bash
Nmap scan report for futurevera.thm (<IP>)
Host is up (0.14s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 9d:50:8b:05:89:56:85:b5:12:27:36:1c:d9:24:9e:74 (RSA)
|   256 59:ef:a5:1c:6f:c3:72:84:e5:34:4c:1d:1d:ff:58:e9 (ECDSA)
|_  256 2c:e7:f8:0d:e7:10:02:82:fa:5c:24:61:e9:83:d0:d8 (ED25519)
80/tcp  open  http     Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Did not follow redirect to https://futurevera.thm/
|_http-server-header: Apache/2.4.41 (Ubuntu)
443/tcp open  ssl/http Apache httpd 2.4.41 ((Ubuntu))
|_http-title: FutureVera
|_http-server-header: Apache/2.4.41 (Ubuntu)
| ssl-cert: Subject: commonName=futurevera.thm/organizationName=Futurevera/stateOrProvinceName=Oregon/countryName=US
| Not valid before: 2022-03-13T10:05:19
|_Not valid after:  2023-03-13T10:05:19
| tls-alpn: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 110/tcp)
HOP RTT       ADDRESS
1   74.46 ms  10.14.0.1
2   103.49 ms futurevera.thm (<IP>)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Aug 28 07:23:42 2025 -- 1 IP address (1 host up) scanned in 22.95 seconds
```

Next, I scanned for **directories and files** but I couldn't find anything. So, I started looking for **Sub-domains**, and for this I used **theHarvester tool**.
#### theHarvester
```bash
[*] Hosts found: 9
---------------------
.futurevera.thm
Blog.futurevera.thm
FUZZ.futurevera.thm
blog.futurevera.thm
payroll.futurevera.thm
portal.futurevera.thm
secret************.support.futurevera.thm
space.futurevera.thm
support.futurevera.thm
```

I found a few **sub-domains**, `blog.futurevera.thm`,  `support.futurevera.thm`, and `secrethelpdesk934752.support.futurevera.thm`. I visited those **sub-domains** but I couldn't find anything. Then, I visited them again on `port 80` and found the flag.

![[TakeOver.png]]
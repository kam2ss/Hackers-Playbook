Enumeration helps identify a target's vulnerabilities by gathering as much information as possible about it. 
### Directory Scanner (dir_scanner)
The module `auxiliary/scanner/http/dir_scanner` scans **one host** or a **complete network range** for any interesting directories in a directory path. 
```bash
msf > auxiliary/scanner/http/dir_scanner
msf auxiliary(dir_scanner) > set RHOSTS [IP address]
RHOSTS => [IP address]
msf auxiliary(dir_scanner) > set RPORT [port]
RPORT => [port]
msf auxiliary(dir_scanner) > run
```

Once the scan is complete, msfconsole will list the found directories on the target server, including the directory name (such as `/example/` and `/manager/`) and webpage status (such as 301 and 222).
```plaintext
http://[IP address]:[port]/example/ 301 [IP address]
http://[IP address]:[port]/manager/ 222 [IP address]
```
The `/manager/` directory is capable of being brute forced by the Tomcat MGR Login module.

### Tomcat MGR Login
You can try and log in to a Tomcat Manager Application instance by brute forcing with a username and password combination list using the module `auxiliary/scanner/http/tomcat_mgr_login`.
![Options for the module 'tomcat_mgr_login'.](https://il-labforge-assets.origin.immersivelabs.team/uploads/tdM4w3A1xno3DrhaKtoJolpcIUuFJxvMHvI0wXQMbZ8.png)

Here, if you keep the default username and password files, set the fields `RHOSTS` and `RPORT` to the target, and run the command, any successful default sets of Tomcat credentials will appear in the output.

You can also give this module a directory path to a text file of username and password combinations in the field `USERPASS_FILE`. This module will then attempt to brute force log in on target machines rather than the default options.

### SMB Login Check
Alternatively, if you have a valid username and password combination, a Metasploit scan can reveal access information. The SMB Login Check Scanner connects to various hosts to determine where the username and password can gain you access on target machines.
```bash
msf > use auxiliary/scanner/smb/smb_login
```

Enter the username and password you want to test against your target IP address(es).
```bash
msf auxiliary(smb_login) > set RHOSTS [IP]
RHOSTS => [IPs]
msf auxiliary(smb_login) > set SMBUser [username]
SMBUser => [username]
msf auxiliary(smb_login) > set SMBPass [password]
SMBPass => [password]
msf auxiliary(smb_login) > set THREADS [number of threads]
THREADS => [number of threads]
msf auxiliary(smb_login) > run
```

Like the Tomcat MGR Login module, this module can also be given a list of usernames and passwords to brute force the login on target machines.

Here are some example text files containing usernames and passwords:
**Usernames.txt: User, Admin, Administrator, Guest, Default, [USER]**  
**Passwords.txt: Password, ChangeMe, Guest, 12345, Admin, [PASSWORD]**

Check the content against the target system by checking for the fields `USER_FILE` and `PASS_FILE` and set the options accordingly.
```bash
msf auxiliary(smb_login) > set USER_FILE /root/users.txt
msf auxiliary(smb_login) > set PASS_FILE /root/passwords.txt
```

It's worth noting that this scan will show as a failed login attempt in the event log of each Windows box it scans. This is a **loud** method of scanning as it can be traced, a contrast to the `scanner/portscan/syn` module we previously looked at.
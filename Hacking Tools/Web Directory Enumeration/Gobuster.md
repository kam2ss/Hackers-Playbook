#### What is Gobuster?
Fast and lightweight directory brute-forcer written in Go.
  
#### How it works? 
Uses wordlists to fuzz directory and file names over HTTP/S, DNS, etc. It is **Best for Speed-focused scans**, works great over VPN or during CTFs.

#### Usage
```bash
gobuster dir -u http://example.com/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
   
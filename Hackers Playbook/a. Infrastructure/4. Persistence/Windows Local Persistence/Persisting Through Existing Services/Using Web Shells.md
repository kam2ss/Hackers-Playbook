#### What is a Web Shell?
Instead of using Windows features for persistence, attackers can exploit existing services - like **web servers** - to maintain access. A common method is uploading a **web shell (e.g., an `.aspx` file)** into the server's ***webroot directory (default: `c:\inetpub\wwwroot`)***.

This grants us access with the privileges of the ***user in IIS***, which is by default `iis apppool\defaultapppool`. Even though this user is unprivileged, it has the ***special `SeImpersonatePrivilege`***, providing as easy way to ***escalate to the Administrator*** using various known exploits.

#### Using Web Shells
1. Download an ASP.NET web shells from [here](https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmdasp.aspx). 
2. Transfer it to the victim machine and move it into the webroot (`c:\inetpub\wwwroot`)
	```
	move shell.aspx C:\inetpub\wwwroot
	```

3. If access denied error, just grant full permissions on the file
```
	icacls shell.aspx /grant Everyone:F
```

4. We can then run commands from the web server by pointing to the URL from ***Kali***:
	`http://<victim_ip>/shell.aspx`
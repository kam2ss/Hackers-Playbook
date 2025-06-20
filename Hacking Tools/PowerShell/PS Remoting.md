### PowerShell Remoting
PowerShell Remoting is a native ***Windows remote command execution*** feature built on top of the Windows Remote Management (WinRM) protocol. It works similarly to SSH for accessing remote terminals on Unix-based systems.  

It should be noted, however, that for a user to access a remote host using this technique, they must be part of the **Remote Management Users** group and both the host and the client must be set as **Trusted Host** for each other.

### Usage
Using PowerShell Remoting is not a complex task. Generally there are two commands that will be required when working with this technology.

1. **Single commands:** 
    These are generally useful for system administrators, allowing them to execute commands on multiple systems from a script. The command looks like this:  
    `Invoke-Command –ComputerName <IP/Hostname> -Credential <Username/PSCredential> -ScriptBlock {<The command to be run>}`
    
2. **PowerShell Sessions:**
    This is used to control a Windows system remotely from an interactive PowerShell session. This is useful to sysadmins and pen-testers when administrative access is gained for a host. The command looks like this:  
    `Enter-PSSession –ComputerName <IP/Hostname> -Credential <Username/PSCredential>`

### Examples
```
# Single command 
Invoke-Command -ComputerName IP -Credential username -ScriptBlock {net user administrator}
```
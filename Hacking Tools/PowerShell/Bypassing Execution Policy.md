### Execution policies
There are seven execution policies that can be set for PowerShell by the machine administrator:

- **All Signed** – All scripts and configuration files must be signed by a trusted publisher.
- **Bypass** – Nothing is blocked and there are no warnings or prompts.
- **RemoteSigned** – All scripts and configuration files downloaded from the Internet must be signed by a trusted publisher.
- **Restricted** – Doesn't load configuration files or run scripts.
- **Default** – Sets the default execution policy. **Restricted** for Windows clients or **RemoteSigned** for Windows servers.
- **Undefined** – No execution policy is set for the scope. If the execution policy in all scopes is **Undefined**, the effective execution policy is **Restricted**.
- **Unrestricted** – Loads all configuration files and runs all scripts. If you run an unsigned script that was downloaded from the Internet, you're prompted for permission before it runs.

### Bypassing the restricted policy
There are multiple ways to bypass the **Restricted** execution policy in PowerShell. Five different methods that can bypass the execution policy will be presented in this lab.

1. **‘Bypass’ execution policy flag.** This technique involves spawning a new PowerShell process, setting the execution policy to Bypass for that scope and passing the script as an argument. The command looks like this:  
    `**PS >** powershell.exe -ExecutionPolicy Bypass -File .\script.ps1`
    
2. **Type and pipe.** Although scripts are disabled, single commands still work, so another technique is to simply read the script and pipe it into the PowerShell executable.  
    `**PS >** type .\script.ps1 | powershell.exe -noprofile -`
    
3. **The copy and the paste** (only in interactive mode). This generally works best for small scripts. As above, this technique works because commands are pasted straight into the console instead of being run from the script itself.
 
4. **The base64 encoded parameter.** PowerShell offers the ability to run commands encoded as base64. To do this, you must encode the contents of the file and pass the resulting string to the `-EncodedCommand` switch of PowerShell.  
    `**PS** **>** $commands = Get-Content script.ps1 -Raw`  
    `**PS >** $bytes = [System.Text.Encoding]::Unicode.GetBytes($commands)`  
    `**PS >** $encodedCommand = [Convert]::ToBase64String($bytes)`  
    `**PS** **>** powershell.exe -EncodedCommand $encodedCommand`
    
5. **Authorisation Manager → NULL**. The following technique will replace the Authorisation Manager with **null** for the current session. Thus, the execution policy will become **Unrestricted** for the remainder of the session. This does not affect any global configuration.  
    `**PS** **>** function Disable-ExecutionPolicy {($ctx = $executioncontext.gettype().getfield("_context","nonpublic,instance").getvalue( $executioncontext)).gettype().getfield("_authorizationManager","nonpublic,instance").setvalue($ctx, (new-object System.Management.Automation.AuthorizationManager "Microsoft.PowerShell"))}`  
    `**PS** **>** Disable-ExecutionPolicy`  
    `**PS** **>** .\script.ps1`

## The restricted policy

In most cases, administrators will harden machines and will set the execution policy to **Restricted**. When trying to run scripts, it will have the below behaviour.

![](https://il-labforge-assets.origin.immersivelabs.team/uploads/_6z1UFdNzI_lh10qxBtL6wdaWzWbTRXDq1Qu9utSDr8.png)

When trying to run the script, it can be seen that an error is raised and the script is blocked by the execution policy. When running the command `Get-ExecutionPolicy -List`, we are presented with the execution policies for all scopes; it can be seen that for the scope LocalMachine, the execution policy is set to **Restricted**.


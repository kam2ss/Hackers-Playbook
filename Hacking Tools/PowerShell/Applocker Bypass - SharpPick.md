### PowerShell vs powershell.exe
Powershell.exe is a process that hosts the System.Management.Automation.dll, which is essentially the actual PowerShell as we know it. This being said, there are still ways to execute PowerShell commands in cases when powershell.exe is blocked by applications like AppLocker. This can be done by creating an executable which implements functions from System.Management.Automation.dll.

### SharpPick
SharpPick is an open source project created by PowerShellEmpire; you can find the code [here](https://github.com/PowerShellEmpire/PowerTools/tree/master/PowerPick/SharpPick). The function that executes PowerShell commands is quite trivial to implement. It first initialises a RunspaceInvoker and a Pipeline, as shown below.
```cs
Runspace runspace = RunspaceFactory.CreateRunspace();
runspace.Open();
RunspaceInvoke scriptInvoker = new RunspaceInvoke(runspace);
Pipeline pipeline = runspace.CreatePipeline();
```

It then adds user input as an argument, calls the Invoke method and saves the output in a `PSObject` collection.
```cs
pipeline.Commands.AddScript(cmd);
pipeline.Commands.Add("Out-String");
Collection<PSObject> results = pipeline.Invoke();
runspace.Close();
```

Finally, the function iterates through the PSObject collection, adds the results to a StringBuilder object and returns the resulting String.
```cs
StringBuilder stringBuilder = new StringBuilder();
foreach (PSObject obj in results)
{
   stringBuilder.Append(obj);
}
return stringBuilder.ToString().Trim();
```

The above function does all the PowerShell code execution. The rest of the program just contains argument parsing and helps display code. Knowing how to implement this kind of program yourself might come in handy in situations where internet access is not available on a host.

### Actual Process
```
# Use MSBuild at this location to compile the SharpPick tool
cd C:\Windows\Microsoft.NET\Framework64\v4.0.30319\
MSBuild.exe C:\Users\IMLUser\Desktop\SharpPick\SharpPick.cproj

# Once the compilation is succeded, Check for the 'SharpPick.exe' location 
# that was created during the compilation process
cd C:\Users\IMLUser\Desktop\SharpPick\bin\x86\Debug\
SharpPick.exe -c "Get-Module -ListAvailable | Select -Property Name | Out-File C:\Users\IMLUser\Desktop\Modules.txt"
```

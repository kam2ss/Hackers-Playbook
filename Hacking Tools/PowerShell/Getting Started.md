### What is PowerShell?
Windows PowerShell is a command line shell and scripting language primarily designed for task automation and configuration management. 

In 2016, Microsoft made PowerShell Core an open source and cross-platform project, with support for Windows, Linux and MacOS. The commands are the same whether you're using Windows PowerShell or PowerShell Core.

### Cmdlets (Command-lets)
PowerShell uses ***cmdlets***: small, lightweight modules designed to run tasks in place of traditional commands. Cmdlets return output as an object (or an array of objects) and can transfer this data to other cmdlets using pipes.

Cmdlets always contain a verb and noun separated by a dash in their name. For example, `Get-Process` and `Set-Location`. Here are some of the cmdlet verbs that are used:
```powershell
Get-  # get something
Set-  # define something
Start-  # run something
Stop- # stop something that's running
Out-  # output something
New-  # create something new
```

To run a PowerShell cmdlet, simply type the cmdlet name and hit enter. For example, you can type `Get-Process` to see the running processes. To see a list of all commands available to you, type `Get-Command`.

You can get help using a specific cmdlet by using the `Get-Help` cmdlet (e.g., Get-Help Get-Process) or see all available help topics by using `Get-Help *`.

### Aliases
Aliases in PowerShell provide an alternative name for running the underlying cmdlet. There are several shorthand aliases built in; for example, the `dir` command will run `Get-ChildItem`.

All aliases can be viewed by running the `alias` command, and specific aliases can be viewed by specifying them; for example, `alias dir`.

### Moving around
Commands for changing directories and viewing directory listings are the same as the Linux command line and Windows command prompt. Commands such as `cd`, `dir`, `type`, etc, will still work.

These commands are just aliases to the new PowerShell cmdlets such as `Set-Location` for `cd` and `Get-ChildItem` for `dir`.

### Parameters
In the same way that traditional commands can be provided with command line arguments, PowerShell cmdlets can be given optional parameters.

The parameters can be specified by prefixing them with a dash (-); for example, to show hidden files in a directory, you can run `Get-ChildItem -Hidden`.

To see all the parameters available for a cmdlet, run the cmdlet together with the Get-Member cmdlet, e.g., `Get-Process | Get-Member`.

### Pipes
A pipe is used to pass data from one cmdlet to another. Pipes can be used to sort the output of the cmdlet and to redirect output to a file. 

In the following example, a pipe is used to sort the output of the Get-Process cmdlet by ID:
```powershell
Get-Process | Sort-Object -Property ID
```

### Variables
Variables in PowerShell are prefixed with a dollar symbol ($) and assigned by stating the variable name that's followed by an equals sign (=) and the desired value.

For example, we can set a variable called foo to a value by typing `$foo = ‘bar’`. Variables can be updated by setting them again and viewed by calling the variable name.
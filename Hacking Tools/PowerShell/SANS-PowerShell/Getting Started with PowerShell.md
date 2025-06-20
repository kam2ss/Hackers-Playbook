### Verb-Noun
```
# Lists of approved verbs
Get-Verb

# Returns error, there's no defined list of PowerShell nouns
Get-Noun
```

### Get-Help for Instructions
```
# Displays all information at once
Get-Help Get-Process

# Display syntax, a description, online resources and more for 'Get-Process' command
Get-Help -Examples Get-Process

# Show you several examples for each command
Get-Help about_Core_Commands

# Displays one screen at a time
help Get-Process
```

### Information about PowerShell Commands
```
# 'Get-Command' is a cmdlet. 
# Getting info about the 'Start-Process' command
Get-Command -Name Start-Process
```

### Find commands with Noun
```
# Using 'Get-Command' with '-Noun' can find all of the commands that 
# work with a specific noun.
Get-Command -Noun Volume
```

### Aliases
```
# Check alias
alias dir

# Check all the aliases associated with 'Get-ChildItem'
Get-Alias -Definition Get-ChildItem
```

### Write
```
Write-Host 'Hello, PowerShell!'
```

### Auto-Completion
```
# Use tab to auto-complete
Get-ChildItem \U

# Get the first option with the tab
get-ne
```

### Process
```
# Display the running processes
Get-Process 

# Starts the process specified
Start-process -Name <process name> 
Start-Process <program name>

# Stops the process specified
Stop-process -Name <process name> 
```

### SC
There is the `Service Control` utility for managing windows services. PowerShell uses `sc` as well as an alias. So, to avoid the name conflict:
In PowerShell, if you don't specify a path, then it will run commands in this order: `aliases, functions, cmdlets, then external programs`. E.g. `sc query`
```
# To run a local Windows executable, include '.exe'
sc.exe query

# For PowerShell 'Set-Content'
sc query
```

### Remove-Item
```
Get-ChildItem -Name *process* | Remove-Item
```

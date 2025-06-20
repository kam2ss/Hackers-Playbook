#### To display all cmdlets
```
Get-Command

# To know which cmdlets are available for a specific noun
Get-Command -Noun Process
```

#### To show all aliases
```
alias *

# To show gcm alias
alias gcm

# To show dir alias
alias dir
```

## Find files in the system
```
# To list all text files in the user's directory
get-childitem -recurse -filter "*.txt"

# Find '.txt' file under the user's home directory
Get-ChildItem -Path $HOME -Recurse -Filter '*.txt'
```

## Count
```
# To count '.png' files in the directory and its sub-dir
(get-childitem -Path '/home/iml-user' -recurse -include "*.png").count
```

### To write data to a read-only file
```
Set-Content -Path "C:\path\to\your\file.txt" -Value "Your data" -Force
```

#### Other cmdlet to add content to a file without overwriting the existing content
```
# To find other cmdlet with the content
get-help *-content

Add-Content -Path "C:\path\to\your\file.txt" -Value "Your data"
```

#### Create a new file with contents 'Hello There'
```
Set-Content test.txt -value "Hello There"
```

## JSON Files
#### Convert a .txt file into JSON and save it
```
# This 'ConvertTo-Json' cmdlet can convert to a JSON file but this cmdlet works with objects, not with 
# the raw text files. So, you would first need to read the file and parse it into an object that 
# PowerShell can understand.

# Read the file into an array, one line per element
$ipaddresses = Get-Content -Path './ipaddresses.txt'

# Convert the array to a JSON string
$json = $ipaddresses | ConvertTo-Json

# Write the JSON string to a file
$json | Out-File -FilePath 'ipaddresses.json'
```

#### To read the first content from a JSON file
```
(Get-Content -Path 'ipaddresses.json' -Raw | ConvertFrom-Json)[0]
```

## html Files
```
Get-Host | ConvertTo-Html | Out-File -FilePath host.html
```

## Modules
```
#### Count all available modules
(Get-MOdule -ListAvailable).count

#### To count modules that are currently imported
(Get-Module).count

#### To import modules 
Import-Module -Name 'Posh-SYSLOG'

#### To check the version of an installed module
(Get-Module -ListAvailable | Where-Object {$_.Name -eq 'Posh-SYSLOG'}).Version

#### Remove a module
Remove-Module -Name 'Pester'
```

## EventLog Files
```
# Counting system logs that were generated on a specific date. To limit the output, 
# we have to provide the before and after date.
Get-EventLog -LogName "System" -After (Get-Date -Year 2018 -Month 5 -Day 2 -Hour 0 -Minute 0 -Second 0) -Before (Get-Date -Year 2018 -Month 5 -Day 3 -Hour 23 -Minute 59 -Second 59) | Measure-Object
```

## Execution Policy
```
#### List all execution policies
Get-ExecutionPolicy -List

#### Bypass Execution Policy
powershell -ep bypass

#### Bypass the policy to run a script
powershell.exe -ExecutionPolicy Bypass -File .\script.ps1
```
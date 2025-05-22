***PowerShell passes `Object` in the pipeline, not just text.***

***An `object` is a collection of code (methods) and data properties that represents the item.***

When you run a PowerShell command and send the output in a pipeline, you are sending an object or a collection of objects to the next step in the pipeline. these objects can include more information than what you see by default.

### Sorting
```
# displayed the services in service name order, alphabetically sorted
Get-Serice | Sort-Object -Descending

# Adding 'more'
Get-Serice | Sort-Object -Descending | more

# Retrives all the process and sorting them by their name or ID
Get-Serice | Sort-Object -Property Name
Get-Serice | Sort-Object -Property ID

# Get the unique list of processes
Get-Serice | Sort-Object -Property Name -Descending -Unique
```

### Redirection
When you use the redirection operator `>` or `Out-File`, PowerShell takes the pipeline output and creates a little-endian UTF-16 Unicode text file with a `Byte Order Mark (BOM)` at the beginning of the file. These files look like text files, but `they aren't plain ASCII files`.
```
# Using redirection
Get-Process > processes.txt

# Using a pipeline
Get-Process | Out-File -FilePath processes.txt
```

To `create a plain ASCII text file`, use Out-File with the -Encoding ASCII.
```
Get-Process | Out-File -FilePath processes.txt
```

### Tee
```
# When you want the output to go to the screen and go to a file.
Get-Process | Tee-Object -FilePath processes4.txt
```

### Retrieve Information
```
# Using the 'select-object' to get more info about threads
Get-Process lsass | Select-Object -Property Threads

# Getting more info
Get-Service -Name eventlog | Select-Object -Property Status, Name, DisplayName, StartType

# Exporting the results to an HTML report using 'convertTo-HTML'
Get-Process | Select-Object -Property Name, Id, Path, CPU, WorkingSet64 | ConvertTo-Html | Out-File processes.html
```

### Summary Information
`Measure-Object` returns information about the numeric properties of the objects received in the pipeline.
```
Get-Process lsass | Measure-Object
```

### Checking Object-Member
`Get-Members` returns information about the object properties and methods received in the pipeline.
```
# Looking at the object members
Get-Process -Name rexplorer | Get-Membere
```

### Table-view
```
Get-Service -Name eventlog | Select-Object -Property Status, Name, DisplayName, StartType | Format-Table
```

### Parameter Binding
PowerShell command matches the input you supply in the pipeline to a designated parameter. `winrm` is a string, but in the pipeline it becomes an object. 
```
'winrm' | Get-Member
```

### Where-Object
`Where-Object` preforms an action on each object in the collection. This is often used for filtering results in the pipeline.
```
Get-Service | Where-Object -Property Status -EQ Running
```

### Partial String Matching
```
# 'Like' operator to do partial string matching
Get-Service | Where-Object -Property DisplayName -Like 'Application*'

# Match any path with a directory that includes 'temp' in the name
Get-Process | Where-Object -Property Path -Like '*temp*'

# Retrieving multiple details
Get-Process | Where-Object -Property Path -Like '*temp*' | Select-Object -Property Name, ID, Path, StartTime
```
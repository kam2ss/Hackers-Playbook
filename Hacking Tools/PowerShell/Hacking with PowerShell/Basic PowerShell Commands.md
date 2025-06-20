### Using Get-Help
`Get-Help` displays information about a cmdlet.

**Get help with a particular command:**
```powershell
Get-Help Command-Name
```

```powershell
Get-Help Get-Command -Examples
```

### Using Get-Command
**Get all the `cmdlets` installed on the computer:**
```powershell
Get-Command 
```

**This `cmdlet` allows for Pattern Matching:**
```powershell
# Get-Command Verb-*
Get-Command New-*

# or

# Get-Command *-Noun
Get-Command *-Warning
```

### Object Manipulation
A major difference compared to other shells is that PowerShell ***passes an object to the next cmdlet instead of passing text or string*** to the command after the pipe. Like every object in object-oriented frameworks, an object will contain methods and properties.

A method can be ***functions that can be applied to output from the cmdlet***, and the ***properties could be variables in the output from a cmdlet***.

- use the pipeline `|` to pass objects between cmdlets
- using specific object `cmdlets` to extract information

**View the Members for `Get-Command`**
```powershell
Get-Command | Get-Member -MemberType Method
```

### Creating Objects From Previous Cmdlets
One way of manipulating objects is pulling out the properties from the output of a cmdlet and creating a new object. This is done using the `Select-Object` cmdlet.

**Listing the directories and just selecting the Mode and the Name**
```powershell
Get-ChildItem | Select-Object -Property Mode, Name
```

### Filtering Objects
You can do this using the `Where-Object` to filter based on the value of properties.

**The general format for using this cmdlet is:**
```powershell
# Older and more limited
Verb-Noun | Where-Object -Property PropertyName -operator Value
```
or
```powershell
# Modern and more flexible
Verb-Noun | Where-Object {$_.PropertyName -operator Value}
```
The second version uses the `$_` operator to iterate through every object passed to the `Where-Object` cmdlet.

- **Common Operators :**
	- `-Contains:` Checks if property contains a specific value
	- `-EQ:` Equals
	- `-GT:` Greater Than

Note: ***PowerShell is quite sensitive, so don't put quotes around the command!***

**Example:**
```powershell
Get-Service | Where-Object -Property Status -eq Stopped
```

### Sort-Object
You do this by pipe-lining the output of a cmdlet to the `Sort-Object` cmdlet.

The format of the command is:
`Verb-Noun | Sort-Object`

**Sorting the List of Directories:**
```powershell
Get-ChildItem | Sort-Object
```
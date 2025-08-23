#### PowerView
PowerView is a PowerShell tool used to gain network situational awareness on Windows domains. PowerView re-implements the old Windows command-line like `net user`, `net group`, etc in PowerShell.

**To get the syntax of a function, you can use the following:**
```powershell
Get-Command <Name of the function> -Syntax
```

#### Helpful functions
- `Get-NetUser:` returns all objects or a user specified as the first parameter of the function. 
- `Invoke-UserFieldSearch:` searches user fields for sensitive keywords. `-Term` will search for specific term, or `-Field` will search in a specific field.
- `Invoke-FindLocalAdminAccess:` checks the whole domain for machines where this user has local administrator access while running under a domain user.
- `Invoke-ShareFinder:` lists all reachable shares including hidden ones.
- `Invoke-FileFinder:` searches all accessible shares for files containing sensitive data. `-Term` switch can make it look for specific keywords.

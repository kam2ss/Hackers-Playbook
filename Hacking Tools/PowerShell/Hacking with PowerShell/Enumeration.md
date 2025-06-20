The fist step after you have gained initial access would be to enumerate. We'll be enumerating the following:
- users
- basic networking information
- file permissions
- registry permissions
- scheduled and running tasks
- insecure files

### Enumerate Users
**Local Users**
```powershell
Get-LocalUser

# Show all properties
Get-LocalUser | Select-Object *
```

**Domain Users**
```powershell
Get-ADUser -Filter * -Properties *
```
Note: Requires the Active Directory module (part of RSAT tools)

**Get specific User**
```powershell
Get-ADUser USERNAME -Properties *
```


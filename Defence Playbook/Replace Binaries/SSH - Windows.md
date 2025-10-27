### Process highlighting how to replace a Vulnerable SSH binary
#### Windows Environment
##### 1. If we have an access to the Internet
Updating the system to the latest one should be able to fix that. However, confirm that with a `nmap` scan after the updating has completed.

##### 2. If we don't have an access to the Internet
- Install `openssh` on your machine with the internet
```
https://github.com/PowerShell/Win32-OpenSSH/releases
```

- Transfer the file over to the vulnerable machine
```
scp <openssh.zip> administrator@<target-ip>:
```

- `Unzip` the `openssh.zip` file in the target machine.
- Stop `ssh` service
```
Stop-Service sshd
```

- Find the path of the old `ssh` 
```
Get-WmiObject Win32_Service -Filter "Name='sshd'" | Select-Object Name, PathName
```

- Backup old `ssh` executables (Before replacing it with the new one)
```
Copy-Item 'C:\path\to\old\ssh" "C:\path\to\old\ssh_backup" -Recurse
```

- Copy the new `ssh` binaries to the system folder or where the old `ssh` was
```
Copy-Item 'C:\path\to\new\ssh" "C:\Windows\System32\OpenSSH-Win64" -Recurse -Force
```

- Start the `ssh` service
```
Start-Service sshd
```

##### 3. Perform an `Nmap` scan to confirm the `ssh` is updated
```
nmap -A -p22 <target-ip>
```

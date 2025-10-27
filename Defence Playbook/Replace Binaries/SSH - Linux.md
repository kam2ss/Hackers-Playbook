### Process highlighting how to replace a Vulnerable SSH binary
#### Linux Environment
##### 1. If we have an access to the Internet
```
sudo apt update && sudo apt upgrade
```

##### 2. If we don't have an access to the internet
- Download the `ssh` in the machine where you have access to the internet
```
sudo apt install openssh-server -y
```

- Transfer the `ssh` binary to the target machine
```
scp <openssh.tar.gz> user@<target-ip>:/tmp
```

- Stop the `ssh` service
```
sudo systemctl stop sshd
```

- Backup the existing binary
```
sudo cp /usr/sbin/sshd /usr/sbin/sshd.bak
```

- Backup client binary
```
sudo cp /usr/sbin/ssh /usr/sbin/ssh.bak
```

- Backup configuration directory
```
sudo cp /etc/ssh /etc/ssh_backup_$(date +%F_%H%M)
```

- Extract and install new `ssh`
```
tar -xzf <openssh.tar.gz>
cd <openssh>
```

- Compile and install from source
```
./configure --prefix=/usr --sysconfdir=/etc/ssh
makefile
sudo make install
```

- Restart `ssh` service
```
sudo systemctl start sshd
```

##### 3. Verify using `nmap`
```
nmap -A -p22 <target-ip>
```


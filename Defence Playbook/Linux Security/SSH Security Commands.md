### Securing SSH
- Filter SSH access at the firewall level (iptables)
- Activate Public Key Authentication and Disable Password Authentication

### 1. Installing OpenSSH (client and server)
#### Ubuntu
```
sudo apt update && sudo apt install openssh-server openssh-client
```
#### CentOS
```
sudo dnf install openssh-server openssh-clients
```
#### Connecting to the Server
```
ssh -p 22 username@server_ip   # => Ex: ssh -p 2267 john@192.168.0.100
ssh -p 22 -l username server_ip
ssh -v -p 22 username@server_ip # => verbose
```

### 2. Controlling the SSHd Daemon
```
# checking its status
sudo systemctl status ssh # => Ubuntu
sudo systemctl status sshd # => CentOS
```
#### Stopping the Daemon
```
sudo systemctl stop ssh # => Ubuntu
sudo systemctl stop sshd # => CentOS
```
#### Restarting the Daemon
```
sudo systemctl restart ssh # => Ubuntu
sudo systemctl restart sshd # => CentOS
```
#### Enabling at Boot Time
```
sudo systemctl enable ssh # => Ubuntu
sudo systemctl enable sshd # => CentOS

sudo systemctl is-enabled ssh # => Ubuntu
sudo systemctl is-enabled sshd # => CentOS
```

### 3. Securing the SSHd Daemon
#### Change the config file (/etc/ssh/sshd_config) and then Restart the Server
```
# Disable direct root login
PermitRootLogin no

# Limit Users’ SSH access
AllowUsers stud u1 u2 john

# Other configurations:
ClientAliveInterval 300
ClientAliveCountMax 0
MaxAuthTries 2
MaxStartUps 3
LoginGraceTime 20
```

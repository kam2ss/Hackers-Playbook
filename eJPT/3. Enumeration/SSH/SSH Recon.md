#### Search '--script' and '--script-args'
```
# Search for the --script to use
echo ls /usr/share/nmap/scripts/ssh*

# Search for the --script-args to use for auth
cat /usr/share/nmap/scripts/ssh-auth-methods.nse | grep args
or 
grep 'args' /usr/share/nmap/scripts/ssh-auth-methods.nse
```
#### Find SSH Server's Version
```
nmap -sV IP
```

##### Fetch the Banner using nc and check the SSH Version
```
# To check the version
nc IP PORT

# Provides the Welcome Message
ssh root@IP
```

##### Check the "Encryption-Algorithms"
```
nmap -p22 --script ssh2-enum-algos IP
```

##### Check the ssh-rsa host key
```
nmap -p22 --script ssh-hostkey --script-args ssh_hostkey=full IP
```

##### Find the Authentication method used for user 'admin'
```
nmap -p22 --script ssh-auth-methods --script-args="ssh.user=admin" IP
```

##### Run Command on the Remote Computer
```
nmap -p22 --script ssh-run --script-args="ssh-run.cmd=cat /home/student/FLAG,ssh-run.username=student,ssh-run.password=" IP
```
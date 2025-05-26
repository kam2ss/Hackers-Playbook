#### Operating System and Software
**Distribution and Version**
```bash
cat /etc/issue
cat /etc/*-release
```

**Kernel Version**
```bash
cat /proc/version
uname -a
```

**Applications Installed**
```bash
ls -alh /usr/bin/
ls -alh /sbin/
dpkg -l
```

#### Cronjobs
```bash
`crontab -l`  
`ls -alh /var/spool/cron`  
`ls -al /etc/ | grep cron`  
`ls -al /etc/cron*`  
`cat /etc/cron*`  
`cat /etc/at.allow`  
`cat /etc/at.deny`  
`cat /etc/cron.allow`  
`cat /etc/cron.deny`  
`cat /etc/crontab`  
`cat /etc/anacrontab`  
`cat /var/spool/cron/crontabs/root`
```

#### SUID Binaries
```
find / -perm -g=s -o -perm -4000 ! -type l -maxdepth 3 -exec ls -ld {} \; 2>/dev/null
```

#### Users
```
sudo -l
```











